---
title: CSDL-Spezifikation-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: 642e5977ecbbf0c474cac1ceae19d33a135aa875
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415514"
---
# <a name="csdl-specification"></a><span data-ttu-id="0f046-102">CSDL-Spezifikation</span><span class="sxs-lookup"><span data-stu-id="0f046-102">CSDL Specification</span></span>
<span data-ttu-id="0f046-103">Die konzeptionelle Schemadefinitionssprache (CSDL) ist eine XML-basierte Sprache, die die Entitäten, Beziehungen und Funktionen beschreibt, die ein konzeptionelles Modell einer datengesteuerten Anwendung bilden.</span><span class="sxs-lookup"><span data-stu-id="0f046-103">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="0f046-104">Dieses konzeptionelle Modell kann von der Entity Framework oder WCF Data Services verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-104">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="0f046-105">Die Metadaten, die mit CSDL beschrieben werden, werden vom-Entity Framework verwendet, um Entitäten und Beziehungen, die in einem konzeptionellen Modell definiert sind, einer-Datenquelle zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="0f046-105">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="0f046-106">Weitere Informationen finden Sie unter [SSDL-Spezifikation](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) und [MSL-Spezifikation](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="0f046-106">For more information, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) and [MSL Specification](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span></span>

<span data-ttu-id="0f046-107">CSDL ist die Implementierung des Entity Data Model der Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="0f046-107">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="0f046-108">In einer Entity Framework-Anwendung werden Metadaten des konzeptionellen Modells aus einer CSDL-Datei (geschrieben in CSDL) in eine Instanz der System. Data. Metadata. Edm. EdmItemCollection geladen. der Zugriff ist über Methoden im System. Data. Metadata. Edm. MetadataWorkspace-Klasse.</span><span class="sxs-lookup"><span data-stu-id="0f046-108">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="0f046-109">Entity Framework verwendet Metadaten des konzeptionellen Modells, um Abfragen für das konzeptionelle Modell in Datenquellen spezifische Befehle zu übersetzen.</span><span class="sxs-lookup"><span data-stu-id="0f046-109">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="0f046-110">Der EF-Designer speichert Informationen über das konzeptionelle Modell in einer EDMX-Datei zur Entwurfszeit.</span><span class="sxs-lookup"><span data-stu-id="0f046-110">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="0f046-111">Zur Erstellungszeit verwendet der EF-Designer Informationen in einer EDMX-Datei, um die CSDL-Datei zu erstellen, die zur Laufzeit von Entity Framework benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-111">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="0f046-112">Die verschiedenen Versionen von CSDL werden von XML-Namespaces unterschieden.</span><span class="sxs-lookup"><span data-stu-id="0f046-112">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="0f046-113">CSDL-Version</span><span class="sxs-lookup"><span data-stu-id="0f046-113">CSDL Version</span></span> | <span data-ttu-id="0f046-114">XML-Namespace</span><span class="sxs-lookup"><span data-stu-id="0f046-114">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="0f046-115">CSDL v1</span><span class="sxs-lookup"><span data-stu-id="0f046-115">CSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="0f046-116">CSDL v2</span><span class="sxs-lookup"><span data-stu-id="0f046-116">CSDL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="0f046-117">CSDL v3</span><span class="sxs-lookup"><span data-stu-id="0f046-117">CSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="0f046-118">Zuordnungselement (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-118">Association Element (CSDL)</span></span>

<span data-ttu-id="0f046-119">Ein **Association** -Element definiert eine Beziehung zwischen zwei Entitäts Typen.</span><span class="sxs-lookup"><span data-stu-id="0f046-119">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="0f046-120">Eine Zuordnung muss die Entitätstypen, die in der Beziehung enthalten sind, und die mögliche Anzahl von Entitätstypen an den Enden der Beziehung angeben, die auch als Multiplizität bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-120">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="0f046-121">Die Multiplizität eines Zuordnungs Endes kann über einen Wert von eins (1), NULL oder eins (0.. 1) oder viele (\*) verfügen.</span><span class="sxs-lookup"><span data-stu-id="0f046-121">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="0f046-122">Diese Informationen werden in zwei untergeordneten End-Elementen angegeben.</span><span class="sxs-lookup"><span data-stu-id="0f046-122">This information is specified in two child End elements.</span></span>

<span data-ttu-id="0f046-123">Auf Entitätstypinstanzen an einem Ende einer Zuordnung kann über Navigationseigenschaften oder Fremdschlüssel zugegriffen werden, sofern sie für einen Entitätstyp verfügbar gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-123">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="0f046-124">In einer Anwendung stellt eine Instanz einer Zuordnung eine bestimmte Zuordnung zwischen Instanzen von Entitätstypen dar.</span><span class="sxs-lookup"><span data-stu-id="0f046-124">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="0f046-125">Zuordnungsinstanzen werden in einem Zuordnungssatz logisch gruppiert.</span><span class="sxs-lookup"><span data-stu-id="0f046-125">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="0f046-126">Ein **Association** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="0f046-126">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f046-127">Dokumentation (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-127">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f046-128">Ende (genau 2 Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-128">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="0f046-129">Referenentialeinschränkung (kein oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-129">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="0f046-130">Annotation-Elemente (0 (null) oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-130">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-131">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-131">Applicable Attributes</span></span>

<span data-ttu-id="0f046-132">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **Association** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-132">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="0f046-133">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-133">Attribute Name</span></span> | <span data-ttu-id="0f046-134">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-134">Is Required</span></span> | <span data-ttu-id="0f046-135">value</span><span class="sxs-lookup"><span data-stu-id="0f046-135">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="0f046-136">**Name**</span><span class="sxs-lookup"><span data-stu-id="0f046-136">**Name**</span></span>       | <span data-ttu-id="0f046-137">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-137">Yes</span></span>         | <span data-ttu-id="0f046-138">Der Name der Zuordnung.</span><span class="sxs-lookup"><span data-stu-id="0f046-138">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f046-139">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **Association** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-139">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="0f046-140">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-140">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-141">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-141">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f046-142">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-142">Example</span></span>

<span data-ttu-id="0f046-143">Das folgende Beispiel zeigt ein **Association** -Element, das die Zuordnung **CustomerOrders** definiert, wenn Fremdschlüssel für die Entitäts Typen **Customer** und **Order** nicht verfügbar gemacht wurden.</span><span class="sxs-lookup"><span data-stu-id="0f046-143">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="0f046-144">Die multiplizitätswerte für jedes **Ende** der Zuordnung geben an, dass viele **Bestellungen** einem **Kunden**zugeordnet werden können, aber nur ein **Kunde** kann einem **Auftrag**zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-144">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="0f046-145">Außerdem gibt das **OnDelete** -Element an, dass alle **Bestellungen** , die mit einem bestimmten **Kunden** verknüpft sind und in den ObjectContext geladen wurden, gelöscht werden, wenn der **Kunde** gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-145">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="0f046-146">Das folgende Beispiel zeigt ein **Association** -Element, das die Zuordnung " **CustomerOrders** " definiert, wenn Fremdschlüssel für die Entitäts Typen " **Customer** " und " **Order** " verfügbar gemacht wurden.</span><span class="sxs-lookup"><span data-stu-id="0f046-146">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="0f046-147">Wenn Fremdschlüssel verfügbar gemacht werden, wird die Beziehung zwischen den Entitäten mit einem **referentialeinschränkung** -Element verwaltet.</span><span class="sxs-lookup"><span data-stu-id="0f046-147">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="0f046-148">Ein entsprechendes AssociationSetMapping-Element ist zur Zuordnung dieser Zuweisung zur Datenquelle nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="0f046-148">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
   <ReferentialConstraint>
        <Principal Role="Customer">
            <PropertyRef Name="Id" />
        </Principal>
        <Dependent Role="Order">
             <PropertyRef Name="CustomerId" />
         </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="0f046-149">AssociationSet-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-149">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="0f046-150">Das **AssociationSet** -Element in konzeptioneller Schema Definitions Sprache (CSDL) ist ein logischer Container für Zuordnungs Instanzen desselben Typs.</span><span class="sxs-lookup"><span data-stu-id="0f046-150">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="0f046-151">Ein Zuordnungssatz stellt eine Definition zum Gruppieren von Zuordnungsinstanzen bereit, sodass sie einer Datenquelle zugeordnet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-151">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="0f046-152">Das **AssociationSet** -Element kann über die folgenden untergeordneten Elemente verfügen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="0f046-152">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f046-153">Dokumentation (kein (null) oder ein Element zulässig)</span><span class="sxs-lookup"><span data-stu-id="0f046-153">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="0f046-154">Ende (genau zwei Elemente erforderlich)</span><span class="sxs-lookup"><span data-stu-id="0f046-154">End (exactly two elements required)</span></span>
-   <span data-ttu-id="0f046-155">Annotation-Elemente (0 (null) oder mehr Elemente zulässig)</span><span class="sxs-lookup"><span data-stu-id="0f046-155">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="0f046-156">Das **Association** -Attribut gibt den Zuordnungstyp an, der in einem Zuordnungs Satz enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-156">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="0f046-157">Die Entitätenmengen, die die Enden eines Zuordnungs Satzes bilden, werden mit genau zwei untergeordneten Endelementen angegeben.</span><span class="sxs-lookup"><span data-stu-id="0f046-157">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-158">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-158">Applicable Attributes</span></span>

<span data-ttu-id="0f046-159">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **AssociationSet** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-159">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="0f046-160">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-160">Attribute Name</span></span>  | <span data-ttu-id="0f046-161">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-161">Is Required</span></span> | <span data-ttu-id="0f046-162">value</span><span class="sxs-lookup"><span data-stu-id="0f046-162">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f046-163">**Name**</span><span class="sxs-lookup"><span data-stu-id="0f046-163">**Name**</span></span>        | <span data-ttu-id="0f046-164">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-164">Yes</span></span>         | <span data-ttu-id="0f046-165">Der Name der Entitätssammlung.</span><span class="sxs-lookup"><span data-stu-id="0f046-165">The name of the entity set.</span></span> <span data-ttu-id="0f046-166">Der Wert des **Name** -Attributs darf nicht mit dem Wert des **Association** -Attributs identisch sein.</span><span class="sxs-lookup"><span data-stu-id="0f046-166">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="0f046-167">**Anwalt**</span><span class="sxs-lookup"><span data-stu-id="0f046-167">**Association**</span></span> | <span data-ttu-id="0f046-168">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-168">Yes</span></span>         | <span data-ttu-id="0f046-169">Der vollqualifizierte Name der Zuordnung, von der der Zuordnungssatz Instanzen enthält.</span><span class="sxs-lookup"><span data-stu-id="0f046-169">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="0f046-170">Die Zuordnung muss sich im gleichen Namespace wie der Zuordnungssatz befinden.</span><span class="sxs-lookup"><span data-stu-id="0f046-170">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f046-171">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **AssociationSet** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-171">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="0f046-172">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-172">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-173">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-173">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f046-174">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-174">Example</span></span>

<span data-ttu-id="0f046-175">Das folgende Beispiel zeigt ein **EntityContainer** -Element mit zwei **AssociationSet** -Elementen:</span><span class="sxs-lookup"><span data-stu-id="0f046-175">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="0f046-176">CollectionType-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-176">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="0f046-177">Das **CollectionType** -Element in der konzeptionellen Schema Definitions Sprache (CSDL) gibt an, dass ein Funktionsparameter oder Funktions Rückgabetyp eine Auflistung ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-177">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="0f046-178">Das **CollectionType** -Element kann ein untergeordnetes Element des Parameter-Elements oder des ReturnType (Function)-Elements sein.</span><span class="sxs-lookup"><span data-stu-id="0f046-178">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="0f046-179">Der Typ der Auflistung kann entweder mit dem **Type** -Attribut oder einem der folgenden untergeordneten Elemente angegeben werden:</span><span class="sxs-lookup"><span data-stu-id="0f046-179">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="0f046-180">**CollectionType**</span><span class="sxs-lookup"><span data-stu-id="0f046-180">**CollectionType**</span></span>
-   <span data-ttu-id="0f046-181">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="0f046-181">ReferenceType</span></span>
-   <span data-ttu-id="0f046-182">RowType</span><span class="sxs-lookup"><span data-stu-id="0f046-182">RowType</span></span>
-   <span data-ttu-id="0f046-183">TypeRef</span><span class="sxs-lookup"><span data-stu-id="0f046-183">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="0f046-184">Ein Modell überprüft nicht, ob der Typ einer Auflistung sowohl mit dem **Type** -Attribut als auch mit einem untergeordneten Element angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-184">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-185">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-185">Applicable Attributes</span></span>

<span data-ttu-id="0f046-186">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **CollectionType** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-186">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="0f046-187">Beachten Sie, dass die Attribute **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**und **COLLATIONS** nur auf Auflistungen von **edmsimpletypes**anwendbar sind.</span><span class="sxs-lookup"><span data-stu-id="0f046-187">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="0f046-188">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-188">Attribute Name</span></span>                                                          | <span data-ttu-id="0f046-189">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-189">Is Required</span></span> | <span data-ttu-id="0f046-190">value</span><span class="sxs-lookup"><span data-stu-id="0f046-190">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f046-191">**Typ**</span><span class="sxs-lookup"><span data-stu-id="0f046-191">**Type**</span></span>                                                                | <span data-ttu-id="0f046-192">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-192">No</span></span>          | <span data-ttu-id="0f046-193">Der Typ der Auflistung.</span><span class="sxs-lookup"><span data-stu-id="0f046-193">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="0f046-194">**NULL zulassen**</span><span class="sxs-lookup"><span data-stu-id="0f046-194">**Nullable**</span></span>                                                            | <span data-ttu-id="0f046-195">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-195">No</span></span>          | <span data-ttu-id="0f046-196">**True** (Standardwert) oder **False** abhängig davon, ob die Eigenschaft einen NULL-Wert haben kann.</span><span class="sxs-lookup"><span data-stu-id="0f046-196">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="0f046-197">> In CSDL v1 muss eine Eigenschaft eines komplexen Typs über `Nullable="False"`verfügen.</span><span class="sxs-lookup"><span data-stu-id="0f046-197">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="0f046-198">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="0f046-198">**DefaultValue**</span></span>                                                        | <span data-ttu-id="0f046-199">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-199">No</span></span>          | <span data-ttu-id="0f046-200">Der Standardwert der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="0f046-200">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="0f046-201">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="0f046-201">**MaxLength**</span></span>                                                           | <span data-ttu-id="0f046-202">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-202">No</span></span>          | <span data-ttu-id="0f046-203">Maximale Länge des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="0f046-203">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="0f046-204">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="0f046-204">**FixedLength**</span></span>                                                         | <span data-ttu-id="0f046-205">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-205">No</span></span>          | <span data-ttu-id="0f046-206">**True** oder **false** , abhängig davon, ob der Eigenschafts Wert als Zeichenfolge mit fester Länge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-206">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="0f046-207">**Genauigkeit**</span><span class="sxs-lookup"><span data-stu-id="0f046-207">**Precision**</span></span>                                                           | <span data-ttu-id="0f046-208">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-208">No</span></span>          | <span data-ttu-id="0f046-209">Die Genauigkeit des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="0f046-209">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="0f046-210">**Skalieren**</span><span class="sxs-lookup"><span data-stu-id="0f046-210">**Scale**</span></span>                                                               | <span data-ttu-id="0f046-211">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-211">No</span></span>          | <span data-ttu-id="0f046-212">Die Skalierung des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="0f046-212">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="0f046-213">**SRID**</span><span class="sxs-lookup"><span data-stu-id="0f046-213">**SRID**</span></span>                                                                | <span data-ttu-id="0f046-214">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-214">No</span></span>          | <span data-ttu-id="0f046-215">Verweis Bezeichner für räumliche Systeme.</span><span class="sxs-lookup"><span data-stu-id="0f046-215">Spatial System Reference Identifier.</span></span> <span data-ttu-id="0f046-216">Nur für Eigenschaften räumlicher Typen gültig.</span><span class="sxs-lookup"><span data-stu-id="0f046-216">Valid only for properties of spatial types.</span></span><span data-ttu-id="0f046-217">   Weitere Informationen finden Sie unter [SRID](https://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx) .</span><span class="sxs-lookup"><span data-stu-id="0f046-217">   For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="0f046-218">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="0f046-218">**Unicode**</span></span>                                                             | <span data-ttu-id="0f046-219">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-219">No</span></span>          | <span data-ttu-id="0f046-220">**True** oder **false** , abhängig davon, ob der Eigenschafts Wert als Unicode-Zeichenfolge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-220">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="0f046-221">**Sortierung**</span><span class="sxs-lookup"><span data-stu-id="0f046-221">**Collation**</span></span>                                                           | <span data-ttu-id="0f046-222">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-222">No</span></span>          | <span data-ttu-id="0f046-223">Eine Zeichenfolge, die angibt, welche Sortierreihenfolge in der Datenquelle verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-223">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="0f046-224">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **CollectionType** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-224">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="0f046-225">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-226">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f046-227">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-227">Example</span></span>

<span data-ttu-id="0f046-228">Das folgende Beispiel zeigt eine Modell definierte Funktion, die ein **CollectionType** -Element verwendet, um anzugeben, dass die Funktion eine Auflistung von **Person** -Entitäts Typen zurückgibt (wie mit dem **Element Type** -Attribut angegeben).</span><span class="sxs-lookup"><span data-stu-id="0f046-228">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

``` xml
 <Function Name="LastNamesAfter">
        <Parameter Name="someString" Type="Edm.String"/>
        <ReturnType>
             <CollectionType  ElementType="SchoolModel.Person"/>
        </ReturnType>
        <DefiningExpression>
             SELECT VALUE p
             FROM SchoolEntities.People AS p
             WHERE p.LastName >= someString
        </DefiningExpression>
 </Function>
```
 

<span data-ttu-id="0f046-229">Das folgende Beispiel zeigt eine Modell definierte Funktion, die ein **CollectionType** -Element verwendet, um anzugeben, dass die Funktion eine Auflistung von Zeilen zurückgibt (wie im **RowType** -Element angegeben).</span><span class="sxs-lookup"><span data-stu-id="0f046-229">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

<span data-ttu-id="0f046-230">Das folgende Beispiel zeigt eine Modell definierte Funktion, die das **CollectionType** -Element verwendet, um anzugeben, dass die Funktion als Parameter eine Auflistung von **Abteilungs** Entitäts Typen akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="0f046-230">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="0f046-231">ComplexType-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-231">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="0f046-232">Ein **complexType** -Element definiert eine Datenstruktur, die aus **edmsimpletype** -Eigenschaften oder anderen komplexen Typen besteht.</span><span class="sxs-lookup"><span data-stu-id="0f046-232">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span><span data-ttu-id="0f046-233">  Ein komplexer Typ kann eine Eigenschaft eines Entitäts Typs oder eines anderen komplexen Typs sein.</span><span class="sxs-lookup"><span data-stu-id="0f046-233">  A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="0f046-234">Ein komplexer Typ entspricht einem Entitätstyp, in dem von einem komplexen Typ Daten definiert werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-234">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="0f046-235">Es gibt jedoch einige Hauptunterschiede zwischen komplexen Typen und Entitätstypen:</span><span class="sxs-lookup"><span data-stu-id="0f046-235">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="0f046-236">Komplexe Typen weisen keine Identitäten (oder Schlüssel) auf und können daher nicht unabhängig sein.</span><span class="sxs-lookup"><span data-stu-id="0f046-236">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="0f046-237">Komplexe Typen können nur Eigenschaften von Entitätstypen oder anderen komplexen Typen sein.</span><span class="sxs-lookup"><span data-stu-id="0f046-237">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="0f046-238">Komplexe Typen können nicht Teile von Zuordnungen sein.</span><span class="sxs-lookup"><span data-stu-id="0f046-238">Complex types cannot participate in associations.</span></span> <span data-ttu-id="0f046-239">Die Enden einer Zuordnung können kein komplexer Typ sein, daher können Navigationseigenschaften nicht für komplexe Typen definiert werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-239">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="0f046-240">Einer komplexen Typeigenschaft kann kein NULL-Wert zugewiesen werden, obwohl jede skalare Eigenschaft eines komplexen Typs auf NULL festgelegt werden kann.</span><span class="sxs-lookup"><span data-stu-id="0f046-240">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="0f046-241">Ein **complexType** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="0f046-241">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f046-242">Dokumentation (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-242">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f046-243">Property (0 (null) oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-243">Property (zero or more elements)</span></span>
-   <span data-ttu-id="0f046-244">Annotation-Elemente (0 (null) oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-244">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="0f046-245">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **complexType** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-245">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="0f046-246">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-246">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="0f046-247">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-247">Is Required</span></span> | <span data-ttu-id="0f046-248">value</span><span class="sxs-lookup"><span data-stu-id="0f046-248">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f046-249">Name</span><span class="sxs-lookup"><span data-stu-id="0f046-249">Name</span></span>                                                                                                           | <span data-ttu-id="0f046-250">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-250">Yes</span></span>         | <span data-ttu-id="0f046-251">Der Name des komplexen Typs.</span><span class="sxs-lookup"><span data-stu-id="0f046-251">The name of the complex type.</span></span> <span data-ttu-id="0f046-252">Der Name eines komplexen Typs darf nicht dem Namen anderer komplexer Typen, Entitätstypen oder Zuordnungen entsprechen, die sich innerhalb des Bereichs des Modells befinden.</span><span class="sxs-lookup"><span data-stu-id="0f046-252">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="0f046-253">BaseType</span><span class="sxs-lookup"><span data-stu-id="0f046-253">BaseType</span></span>                                                                                                       | <span data-ttu-id="0f046-254">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-254">No</span></span>          | <span data-ttu-id="0f046-255">Der Name eines anderen komplexen Typs, der der Basistyp des zu definierenden komplexen Typs ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-255">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="0f046-256">> Dieses Attribut ist in CSDL v1 nicht anwendbar.</span><span class="sxs-lookup"><span data-stu-id="0f046-256">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="0f046-257">Für komplexe Typen wird Vererbung in dieser Version nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="0f046-257">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="0f046-258">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="0f046-258">Abstract</span></span>                                                                                                       | <span data-ttu-id="0f046-259">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-259">No</span></span>          | <span data-ttu-id="0f046-260">**True** oder **false** (der Standardwert), abhängig davon, ob der komplexe Typ ein abstrakter Typ ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-260">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="0f046-261">> Dieses Attribut ist in CSDL v1 nicht anwendbar.</span><span class="sxs-lookup"><span data-stu-id="0f046-261">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="0f046-262">Komplexe Typen in dieser Version können keine abstrakten Typen sein.</span><span class="sxs-lookup"><span data-stu-id="0f046-262">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="0f046-263">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **complexType** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-263">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="0f046-264">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-264">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-265">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-265">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f046-266">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-266">Example</span></span>

<span data-ttu-id="0f046-267">Das folgende Beispiel zeigt einen komplexen Typ, eine **Adresse**mit den **edmsimpletype** -Eigenschaften " **StreetAddress**", " **City**", " **StateOrProvince**", " **Country**" und " **PostalCode**".</span><span class="sxs-lookup"><span data-stu-id="0f046-267">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="0f046-268">Um die **Adresse** des komplexen Typs (oben) als Eigenschaft eines Entitäts Typs zu definieren, müssen Sie den Eigenschaftentyp in der Entitätstyp Definition deklarieren.</span><span class="sxs-lookup"><span data-stu-id="0f046-268">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="0f046-269">Das folgende Beispiel zeigt die **Address** -Eigenschaft als komplexen Typ für einen Entitätstyp (**Publisher**):</span><span class="sxs-lookup"><span data-stu-id="0f046-269">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

``` xml
 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BooksModel.Address" Name="Address" Nullable="false" />
       <NavigationProperty Name="Books" Relationship="BooksModel.PublishedBy"
                           FromRole="Publisher" ToRole="Book" />
     </EntityType>
```
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="0f046-270">DefiningExpression-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-270">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="0f046-271">Das **DefiningExpression** -Element in konzeptioneller Schema Definitions Sprache (CSDL) enthält einen Entity SQL Ausdruck, der eine Funktion im konzeptionellen Modell definiert.</span><span class="sxs-lookup"><span data-stu-id="0f046-271">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="0f046-272">Zu Überprüfungszwecken kann ein **DefiningExpression** -Element beliebigen Inhalt enthalten.</span><span class="sxs-lookup"><span data-stu-id="0f046-272">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="0f046-273">Entity Framework wird jedoch zur Laufzeit eine Ausnahme auslösen, wenn ein **DefiningExpression** -Element keine gültigen Entity SQL enthält.</span><span class="sxs-lookup"><span data-stu-id="0f046-273">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-274">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-274">Applicable Attributes</span></span>

<span data-ttu-id="0f046-275">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **DefiningExpression** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-275">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="0f046-276">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-276">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-277">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-277">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="0f046-278">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-278">Example</span></span>

<span data-ttu-id="0f046-279">Im folgenden Beispiel wird ein **DefiningExpression** -Element verwendet, um eine Funktion zu definieren, die die Anzahl der Jahre seit der Veröffentlichung eines Buchs zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="0f046-279">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="0f046-280">Der Inhalt des **DefiningExpression** -Elements wird in Entity SQL geschrieben.</span><span class="sxs-lookup"><span data-stu-id="0f046-280">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="0f046-281">Dependent-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-281">Dependent Element (CSDL)</span></span>

<span data-ttu-id="0f046-282">Das **abhängige** Element in konzeptioneller Schema Definitions Sprache (CSDL) ist ein untergeordnetes Element des referentialeinschränkung-Elements und definiert das abhängige Ende einer referenziellen Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="0f046-282">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="0f046-283">Ein **referentialeinschränkung** -Element definiert Funktionen, die einer Einschränkung der referenziellen Integrität in einer relationalen Datenbank ähneln.</span><span class="sxs-lookup"><span data-stu-id="0f046-283">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="0f046-284">So wie eine Spalte (oder Spalten) einer Datenbanktabelle auf den Primärschlüssel einer anderen Tabelle verweisen kann, kann eine Eigenschaft (oder Eigenschaften) eines Entitätstyps auf den Entitätsschlüssel eines anderen Entitätstyps verweisen.</span><span class="sxs-lookup"><span data-stu-id="0f046-284">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="0f046-285">Der Entitätstyp, auf den verwiesen wird, wird als *Prinzipal Ende* der Einschränkung bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="0f046-285">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="0f046-286">Der Entitätstyp, der auf das Prinzipal Ende verweist, wird als *abhängiges Ende* der Einschränkung bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="0f046-286">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="0f046-287">**PropertyRef** -Elemente werden verwendet, um anzugeben, welche Schlüssel auf das Prinzipal Ende verweisen.</span><span class="sxs-lookup"><span data-stu-id="0f046-287">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="0f046-288">Das **abhängige** Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="0f046-288">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f046-289">PropertyRef (ein oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-289">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="0f046-290">Annotation-Elemente (0 (null) oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-290">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-291">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-291">Applicable Attributes</span></span>

<span data-ttu-id="0f046-292">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **abhängige** Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-292">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="0f046-293">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-293">Attribute Name</span></span> | <span data-ttu-id="0f046-294">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-294">Is Required</span></span> | <span data-ttu-id="0f046-295">value</span><span class="sxs-lookup"><span data-stu-id="0f046-295">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="0f046-296">**Rolle**</span><span class="sxs-lookup"><span data-stu-id="0f046-296">**Role**</span></span>       | <span data-ttu-id="0f046-297">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-297">Yes</span></span>         | <span data-ttu-id="0f046-298">Der Name des Entitätstyps am abhängigen Ende der Zuordnung.</span><span class="sxs-lookup"><span data-stu-id="0f046-298">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f046-299">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **abhängige** Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-299">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="0f046-300">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-300">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-301">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-301">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f046-302">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-302">Example</span></span>

<span data-ttu-id="0f046-303">Das folgende Beispiel zeigt ein **referenentialeinschränkungs** -Element, das als Teil der Definition der **PublishedBy** -Zuordnung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-303">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="0f046-304">Die **PublisherID-** Eigenschaft des **Book** -Entitäts Typs bildet das abhängige Ende der referenziellen Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="0f046-304">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="0f046-305">Documentation-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-305">Documentation Element (CSDL)</span></span>

<span data-ttu-id="0f046-306">Das **Documentation** -Element in der konzeptionellen Schema Definitions Sprache (CSDL) kann verwendet werden, um Informationen zu einem Objekt bereitzustellen, das in einem übergeordneten Element definiert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-306">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="0f046-307">Wenn in einer EDMX-Datei das **Documentation** -Element ein untergeordnetes Element eines Elements ist, das als Objekt auf der Entwurfs Oberfläche des EF-Designers angezeigt wird (z. b. eine Entität, eine Zuordnung oder eine Eigenschaft), wird der Inhalt des **Dokumentations** Elements im Visual Studio- **Eigenschaften** Fenster für das Objekt angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0f046-307">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="0f046-308">Das **Documentation** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="0f046-308">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f046-309">**Zusammenfassung**: eine kurze Beschreibung des übergeordneten Elements.</span><span class="sxs-lookup"><span data-stu-id="0f046-309">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="0f046-310">(kein (Null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-310">(zero or one element)</span></span>
-   <span data-ttu-id="0f046-311">**LongDescription**: eine umfassende Beschreibung des übergeordneten Elements.</span><span class="sxs-lookup"><span data-stu-id="0f046-311">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="0f046-312">(kein (Null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-312">(zero or one element)</span></span>
-   <span data-ttu-id="0f046-313">Anmerkungselemente.</span><span class="sxs-lookup"><span data-stu-id="0f046-313">Annotation elements.</span></span> <span data-ttu-id="0f046-314">(keine (Null) oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-314">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-315">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-315">Applicable Attributes</span></span>

<span data-ttu-id="0f046-316">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **Documentation** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-316">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="0f046-317">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-317">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-318">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-318">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="0f046-319">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-319">Example</span></span>

<span data-ttu-id="0f046-320">Das folgende Beispiel zeigt das **Documentation** -Element als untergeordnetes Element eines EntityType-Elements.</span><span class="sxs-lookup"><span data-stu-id="0f046-320">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="0f046-321">Wenn sich der unten aufgeführte Code Ausschnitt im CSDL-Inhalt einer EDMX-Datei befindet, werden die Inhalte der Elemente **Summary** und **LongDescription** im Visual Studio- **Eigenschaften** Fenster angezeigt, wenn Sie auf den `Customer` Entitätstyp klicken.</span><span class="sxs-lookup"><span data-stu-id="0f046-321">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

``` xml
 <EntityType Name="Customer">
    <Documentation>
      <Summary>Summary here.</Summary>
      <LongDescription>Long description here.</LongDescription>
    </Documentation>
    <Key>
      <PropertyRef Name="CustomerId" />
    </Key>
    <Property Type="Int32" Name="CustomerId" Nullable="false" />
    <Property Type="String" Name="Name" Nullable="false" />
 </EntityType>
```
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="0f046-322">End-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-322">End Element (CSDL)</span></span>

<span data-ttu-id="0f046-323">Das **End** -Element in konzeptioneller Schema Definitions Sprache (CSDL) kann ein untergeordnetes Element des Association-Elements oder des AssociationSet-Elements sein.</span><span class="sxs-lookup"><span data-stu-id="0f046-323">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="0f046-324">In jedem Fall ist die Rolle des **End** -Elements anders, und die anwendbaren Attribute unterscheiden sich.</span><span class="sxs-lookup"><span data-stu-id="0f046-324">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="0f046-325">Das End-Element als untergeordnetes Objekt des Association-Elements</span><span class="sxs-lookup"><span data-stu-id="0f046-325">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="0f046-326">Ein **End** -Element (als untergeordnetes Element des **Association** -Elements) identifiziert den Entitätstyp an einem Ende einer Zuordnung und die Anzahl der Entitätstyp Instanzen, die an diesem Ende einer Zuordnung vorhanden sein können.</span><span class="sxs-lookup"><span data-stu-id="0f046-326">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="0f046-327">Zuordnungsenden werden als Teil einer Zuordnung definiert. Eine Zuordnung muss genau zwei Zuordnungsenden aufweisen.</span><span class="sxs-lookup"><span data-stu-id="0f046-327">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="0f046-328">Auf Entitätstypinstanzen an einem Ende einer Zuordnung kann über Navigationseigenschaften oder Fremdschlüssel zugegriffen werden, sofern sie für einen Entitätstyp verfügbar gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-328">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="0f046-329">Ein **Endelement** kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="0f046-329">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f046-330">Dokumentation (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-330">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f046-331">OnDelete (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-331">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="0f046-332">Annotation-Elemente (0 (null) oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-332">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="0f046-333">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-333">Applicable Attributes</span></span>

<span data-ttu-id="0f046-334">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **End** -Element angewendet werden können, wenn es sich um das untergeordnete Element eines **Association** -Elements handelt.</span><span class="sxs-lookup"><span data-stu-id="0f046-334">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="0f046-335">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-335">Attribute Name</span></span>   | <span data-ttu-id="0f046-336">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-336">Is Required</span></span> | <span data-ttu-id="0f046-337">value</span><span class="sxs-lookup"><span data-stu-id="0f046-337">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f046-338">**Typ**</span><span class="sxs-lookup"><span data-stu-id="0f046-338">**Type**</span></span>         | <span data-ttu-id="0f046-339">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-339">Yes</span></span>         | <span data-ttu-id="0f046-340">Der Name des Entitätstyps an einem Ende der Zuordnung.</span><span class="sxs-lookup"><span data-stu-id="0f046-340">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="0f046-341">**Rolle**</span><span class="sxs-lookup"><span data-stu-id="0f046-341">**Role**</span></span>         | <span data-ttu-id="0f046-342">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-342">No</span></span>          | <span data-ttu-id="0f046-343">Der Name für das Zuordnungsende.</span><span class="sxs-lookup"><span data-stu-id="0f046-343">A name for the association end.</span></span> <span data-ttu-id="0f046-344">Wird kein Name angegeben, wird der Name des Entitätstyps am Zuordnungsende verwendet.</span><span class="sxs-lookup"><span data-stu-id="0f046-344">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="0f046-345">**Multiplizität**</span><span class="sxs-lookup"><span data-stu-id="0f046-345">**Multiplicity**</span></span> | <span data-ttu-id="0f046-346">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-346">Yes</span></span>         | <span data-ttu-id="0f046-347">**1**, **0.. 1**oder **\*** , abhängig von der Anzahl der Entitätstyp Instanzen, die sich am Ende der Zuordnung befinden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-347">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="0f046-348">der Wert **1** gibt an, dass genau eine Entitätstyp Instanz am Zuordnungs Ende vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-348">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="0f046-349">**0.. 1** gibt an, dass keine oder nur eine Entitätstyp Instanz am Zuordnungs Ende vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-349">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="0f046-350">**\*** gibt an, dass keine, eine oder mehrere Entitätstyp Instanzen am Zuordnungs Ende vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="0f046-350">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f046-351">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **End** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-351">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="0f046-352">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-352">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-353">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-353">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="0f046-354">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-354">Example</span></span>

<span data-ttu-id="0f046-355">Das folgende Beispiel zeigt ein **Association** -Element, das die Zuordnung **CustomerOrders** definiert.</span><span class="sxs-lookup"><span data-stu-id="0f046-355">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="0f046-356">Die multiplizitätswerte für jedes **Ende** der Zuordnung geben an, dass viele **Bestellungen** einem **Kunden**zugeordnet werden können, aber nur ein **Kunde** kann einem **Auftrag**zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-356">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="0f046-357">Außerdem gibt das **OnDelete** -Element an, dass alle **Bestellungen** , die mit einem bestimmten **Kunden** verknüpft sind und in den ObjectContext geladen wurden, gelöscht werden, wenn der **Kunde** gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-357">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="0f046-358">Das End-Element als untergeordnetes Objekt des AssociationSet-Elements</span><span class="sxs-lookup"><span data-stu-id="0f046-358">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="0f046-359">Das **End** -Element gibt ein Ende eines Zuordnungs Satzes an.</span><span class="sxs-lookup"><span data-stu-id="0f046-359">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="0f046-360">Das **AssociationSet** -Element muss zwei **Endelemente** enthalten.</span><span class="sxs-lookup"><span data-stu-id="0f046-360">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="0f046-361">Die in einem **End** -Element enthaltenen Informationen werden für die Zuordnung eines Zuordnungs Satzes zu einer Datenquelle verwendet.</span><span class="sxs-lookup"><span data-stu-id="0f046-361">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="0f046-362">Ein **Endelement** kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="0f046-362">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f046-363">Dokumentation (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-363">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f046-364">Annotation-Elemente (0 (null) oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-364">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="0f046-365">Anmerkungselemente müssen an alle anderen untergeordneten Elemente angereiht werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-365">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="0f046-366">Annotation-Elemente sind nur in CSDL v2 und höher zulässig.</span><span class="sxs-lookup"><span data-stu-id="0f046-366">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="0f046-367">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-367">Applicable Attributes</span></span>

<span data-ttu-id="0f046-368">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **End** -Element angewendet werden können, wenn es sich um das untergeordnete Element eines **AssociationSet** -Elements handelt.</span><span class="sxs-lookup"><span data-stu-id="0f046-368">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="0f046-369">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-369">Attribute Name</span></span> | <span data-ttu-id="0f046-370">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-370">Is Required</span></span> | <span data-ttu-id="0f046-371">value</span><span class="sxs-lookup"><span data-stu-id="0f046-371">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f046-372">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="0f046-372">**EntitySet**</span></span>  | <span data-ttu-id="0f046-373">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-373">Yes</span></span>         | <span data-ttu-id="0f046-374">Der Name des **EntitySet** -Elements, das ein Ende des übergeordneten **AssociationSet** -Elements definiert.</span><span class="sxs-lookup"><span data-stu-id="0f046-374">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="0f046-375">Das **EntitySet** -Element muss im gleichen Entitäts Container wie das übergeordnete **AssociationSet** -Element definiert werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-375">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="0f046-376">**Rolle**</span><span class="sxs-lookup"><span data-stu-id="0f046-376">**Role**</span></span>       | <span data-ttu-id="0f046-377">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-377">No</span></span>          | <span data-ttu-id="0f046-378">Der Name des Endes des Zuordnungssatzes.</span><span class="sxs-lookup"><span data-stu-id="0f046-378">The name of the association set end.</span></span> <span data-ttu-id="0f046-379">Wenn das **Role** -Attribut nicht verwendet wird, ist der Name des Zuordnungs Satzes Ende der Name der Entitätenmenge.</span><span class="sxs-lookup"><span data-stu-id="0f046-379">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="0f046-380">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **End** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-380">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="0f046-381">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-381">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-382">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-382">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="0f046-383">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-383">Example</span></span>

<span data-ttu-id="0f046-384">Das folgende Beispiel zeigt ein **EntityContainer** -Element mit zwei **AssociationSet** -Elementen, die jeweils zwei **Endelemente** haben:</span><span class="sxs-lookup"><span data-stu-id="0f046-384">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="0f046-385">EntityContainer-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-385">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="0f046-386">Das **EntityContainer** -Element in konzeptioneller Schema Definitions Sprache (CSDL) ist ein logischer Container für Entitätenmengen, Zuordnungs Sätze und Funktions Importe.</span><span class="sxs-lookup"><span data-stu-id="0f046-386">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="0f046-387">Einem Speichermodell-Entitätscontainer wird durch das EntityContainerMapping-Element ein konzeptioneller Modellentitätscontainer zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="0f046-387">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="0f046-388">Ein Speichermodell-Entitätscontainer beschreibt die Struktur der Datenbank: Entitätssätze beschreiben Tabellen, Zuordnungssätze beschreiben Fremdschlüsseleinschränkungen, und Funktionsimporte beschreiben gespeicherte Prozeduren in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="0f046-388">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="0f046-389">Ein **EntityContainer** -Element kann über 0 (null) oder ein Dokumentations Element verfügen.</span><span class="sxs-lookup"><span data-stu-id="0f046-389">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="0f046-390">Wenn ein **Documentation** -Element vorhanden ist, muss es allen **EntitySet**-, **AssociationSet**-und **FunctionImport** -Elementen vorangestellt sein.</span><span class="sxs-lookup"><span data-stu-id="0f046-390">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="0f046-391">Ein **EntityContainer** -Element kann über 0 (null) oder mehrere der folgenden untergeordneten Elemente verfügen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="0f046-391">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f046-392">EntitySet</span><span class="sxs-lookup"><span data-stu-id="0f046-392">EntitySet</span></span>
-   <span data-ttu-id="0f046-393">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="0f046-393">AssociationSet</span></span>
-   <span data-ttu-id="0f046-394">FunctionImport</span><span class="sxs-lookup"><span data-stu-id="0f046-394">FunctionImport</span></span>
-   <span data-ttu-id="0f046-395">Anmerkungselemente</span><span class="sxs-lookup"><span data-stu-id="0f046-395">Annotation elements</span></span>

<span data-ttu-id="0f046-396">Sie können ein **EntityContainer** -Element erweitern, um den Inhalt eines anderen **EntityContainer** -Elements einzubeziehen, das sich im selben Namespace befindet.</span><span class="sxs-lookup"><span data-stu-id="0f046-396">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="0f046-397">Um den Inhalt eines anderen **EntityContainer**einzuschließen, legen Sie im verweisenden **EntityContainer** -Element den Wert des Attributs " **erweitert** " auf den Namen des **EntityContainer** -Elements fest, das Sie einschließen möchten.</span><span class="sxs-lookup"><span data-stu-id="0f046-397">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="0f046-398">Alle untergeordneten Elemente des enthaltenen **EntityContainer** -Elements werden als untergeordnete Elemente des verweisenden **EntityContainer** -Elements behandelt.</span><span class="sxs-lookup"><span data-stu-id="0f046-398">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-399">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-399">Applicable Attributes</span></span>

<span data-ttu-id="0f046-400">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **using** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-400">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="0f046-401">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-401">Attribute Name</span></span> | <span data-ttu-id="0f046-402">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-402">Is Required</span></span> | <span data-ttu-id="0f046-403">value</span><span class="sxs-lookup"><span data-stu-id="0f046-403">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="0f046-404">**Name**</span><span class="sxs-lookup"><span data-stu-id="0f046-404">**Name**</span></span>       | <span data-ttu-id="0f046-405">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-405">Yes</span></span>         | <span data-ttu-id="0f046-406">Der Name des Entitätscontainers.</span><span class="sxs-lookup"><span data-stu-id="0f046-406">The name of the entity container.</span></span>                               |
| <span data-ttu-id="0f046-407">**Erweitern**</span><span class="sxs-lookup"><span data-stu-id="0f046-407">**Extends**</span></span>    | <span data-ttu-id="0f046-408">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-408">No</span></span>          | <span data-ttu-id="0f046-409">Der Name eines anderen Entitätscontainers innerhalb des gleichen Namespaces.</span><span class="sxs-lookup"><span data-stu-id="0f046-409">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f046-410">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **EntityContainer** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-410">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="0f046-411">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-411">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-412">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-412">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f046-413">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-413">Example</span></span>

<span data-ttu-id="0f046-414">Das folgende Beispiel zeigt ein **EntityContainer** -Element, das drei Entitätenmengen und zwei Zuordnungs Sätze definiert.</span><span class="sxs-lookup"><span data-stu-id="0f046-414">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="0f046-415">EntitySet-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-415">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="0f046-416">Das **EntitySet** -Element in der konzeptionellen Schema Definitions Sprache ist ein logischer Container für Instanzen eines Entitäts Typs und Instanzen eines beliebigen Typs, der von diesem Entitätstyp abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-416">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="0f046-417">Die Beziehung zwischen einem Entitätstyp und einem Entitätssatz ist zur Beziehung zwischen einer Zeile und einer Tabelle in einer relationalen Datenbank analog.</span><span class="sxs-lookup"><span data-stu-id="0f046-417">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="0f046-418">Wie eine Zeile definiert ein Entitätstyp einen Satz verknüpfter Daten, und ebenso wie eine Tabelle enthält ein Entitätssatz Instanzen dieser Definition.</span><span class="sxs-lookup"><span data-stu-id="0f046-418">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="0f046-419">Ein Entitätssatz stellt ein Konstrukt zum Gruppieren von Entitätstypinstanzen bereit, damit diese verwandten Datenstrukturen in einer Datenquelle zugeordnet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-419">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="0f046-420">Für einen bestimmten Entitätstyp kann mindestens ein Entitätssatz definiert werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-420">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="0f046-421">Der EF-Designer unterstützt keine konzeptionellen Modelle, die mehrere Entitätenmengen pro Typ enthalten.</span><span class="sxs-lookup"><span data-stu-id="0f046-421">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="0f046-422">Das **EntitySet** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="0f046-422">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f046-423">Documentation-Element (kein (null) oder ein Element zulässig)</span><span class="sxs-lookup"><span data-stu-id="0f046-423">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="0f046-424">Annotation-Elemente (0 (null) oder mehr Elemente zulässig)</span><span class="sxs-lookup"><span data-stu-id="0f046-424">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-425">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-425">Applicable Attributes</span></span>

<span data-ttu-id="0f046-426">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **EntitySet** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-426">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="0f046-427">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-427">Attribute Name</span></span> | <span data-ttu-id="0f046-428">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-428">Is Required</span></span> | <span data-ttu-id="0f046-429">value</span><span class="sxs-lookup"><span data-stu-id="0f046-429">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="0f046-430">**Name**</span><span class="sxs-lookup"><span data-stu-id="0f046-430">**Name**</span></span>       | <span data-ttu-id="0f046-431">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-431">Yes</span></span>         | <span data-ttu-id="0f046-432">Der Name der Entitätssammlung.</span><span class="sxs-lookup"><span data-stu-id="0f046-432">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="0f046-433">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="0f046-433">**EntityType**</span></span> | <span data-ttu-id="0f046-434">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-434">Yes</span></span>         | <span data-ttu-id="0f046-435">Der vollqualifizierte Name des Entitätstyps, für den der Entitätssatz Instanzen enthält.</span><span class="sxs-lookup"><span data-stu-id="0f046-435">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f046-436">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **EntitySet** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-436">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="0f046-437">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-437">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-438">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-438">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f046-439">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-439">Example</span></span>

<span data-ttu-id="0f046-440">Das folgende Beispiel zeigt ein **EntityContainer** -Element mit drei **EntitySet** -Elementen:</span><span class="sxs-lookup"><span data-stu-id="0f046-440">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

<span data-ttu-id="0f046-441">Pro Typ (MEST) können mehrere Entitätssätze definiert werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-441">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="0f046-442">Im folgenden Beispiel wird ein Entitäts Container mit zwei Entitätenmengen für den **Book** -Entitätstyp definiert:</span><span class="sxs-lookup"><span data-stu-id="0f046-442">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="FictionBooks" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="BookAuthor" Association="BooksModel.BookAuthor">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="0f046-443">EntityType-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-443">EntityType Element (CSDL)</span></span>

<span data-ttu-id="0f046-444">Das **EntityType** -Element stellt die Struktur eines Konzepts der obersten Ebene, z. b. einen Kunden oder eine Bestellung, in einem konzeptionellen Modell dar.</span><span class="sxs-lookup"><span data-stu-id="0f046-444">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="0f046-445">Ein Entitätstyp ist eine Vorlage für Instanzen von Entitätstypen in einer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="0f046-445">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="0f046-446">Jede Vorlage enthält die folgenden Informationen:</span><span class="sxs-lookup"><span data-stu-id="0f046-446">Each template contains the following information:</span></span>

-   <span data-ttu-id="0f046-447">Eine eindeutige Bezeichnung.</span><span class="sxs-lookup"><span data-stu-id="0f046-447">A unique name.</span></span> <span data-ttu-id="0f046-448">(Erforderlich.)</span><span class="sxs-lookup"><span data-stu-id="0f046-448">(Required.)</span></span>
-   <span data-ttu-id="0f046-449">Ein Entitätsschlüssel, der von einem oder mehreren Eigenschaften definiert wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-449">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="0f046-450">(Erforderlich.)</span><span class="sxs-lookup"><span data-stu-id="0f046-450">(Required.)</span></span>
-   <span data-ttu-id="0f046-451">Eigenschaften für enthaltene Daten.</span><span class="sxs-lookup"><span data-stu-id="0f046-451">Properties for containing data.</span></span> <span data-ttu-id="0f046-452">(Optional.)</span><span class="sxs-lookup"><span data-stu-id="0f046-452">(Optional.)</span></span>
-   <span data-ttu-id="0f046-453">Navigationseigenschaften, die die Navigation von einem Ende einer Zuordnung zum anderen Ende ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="0f046-453">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="0f046-454">(Optional.)</span><span class="sxs-lookup"><span data-stu-id="0f046-454">(Optional.)</span></span>

<span data-ttu-id="0f046-455">In einer Anwendung stellt eine Instanz eines Entitätstyps ein spezielles Objekt dar, wie etwa einen bestimmten Kunden oder eine Bestellung.</span><span class="sxs-lookup"><span data-stu-id="0f046-455">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="0f046-456">Jede Instanz eines Entitätstyps muss über einen eindeutigen Entitätsschlüssel innerhalb einer Entitätenmenge verfügen.</span><span class="sxs-lookup"><span data-stu-id="0f046-456">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="0f046-457">Zwei Instanzen eines Entitätstyps werden nur dann als gleich betrachtet, wenn sie vom selben Typ sind und die Werte ihrer Entitätsschlüssel übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-457">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="0f046-458">Ein **EntityType** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="0f046-458">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f046-459">Dokumentation (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-459">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f046-460">Key (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-460">Key (zero or one element)</span></span>
-   <span data-ttu-id="0f046-461">Property (0 (null) oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-461">Property (zero or more elements)</span></span>
-   <span data-ttu-id="0f046-462">NavigationProperty (kein Element (null) oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-462">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="0f046-463">Annotation-Elemente (0 (null) oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-463">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-464">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-464">Applicable Attributes</span></span>

<span data-ttu-id="0f046-465">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **EntityType** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-465">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="0f046-466">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-466">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="0f046-467">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-467">Is Required</span></span> | <span data-ttu-id="0f046-468">value</span><span class="sxs-lookup"><span data-stu-id="0f046-468">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f046-469">**Name**</span><span class="sxs-lookup"><span data-stu-id="0f046-469">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="0f046-470">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-470">Yes</span></span>         | <span data-ttu-id="0f046-471">Der Name des Entitätstyps.</span><span class="sxs-lookup"><span data-stu-id="0f046-471">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="0f046-472">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="0f046-472">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="0f046-473">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-473">No</span></span>          | <span data-ttu-id="0f046-474">Der Name eines anderen Entitätstyps, der der Basistyp des Entitätstyps ist, der definiert wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-474">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="0f046-475">**Kter**</span><span class="sxs-lookup"><span data-stu-id="0f046-475">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="0f046-476">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-476">No</span></span>          | <span data-ttu-id="0f046-477">**True** oder **false**, abhängig davon, ob der Entitätstyp ein abstrakter Typ ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-477">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="0f046-478">**OpenType**</span><span class="sxs-lookup"><span data-stu-id="0f046-478">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="0f046-479">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-479">No</span></span>          | <span data-ttu-id="0f046-480">**True** oder **false** , abhängig davon, ob der Entitätstyp ein offener Entitätstyp ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-480">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="0f046-481">> Das **OpenType** -Attribut gilt nur für Entitäts Typen, die in konzeptionellen Modellen definiert sind, die mit ADO.NET-Data Services verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-481">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="0f046-482">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **EntityType** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-482">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="0f046-483">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-483">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-484">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-484">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f046-485">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-485">Example</span></span>

<span data-ttu-id="0f046-486">Das folgende Beispiel zeigt ein **EntityType** -Element mit drei **Eigenschafts** Elementen und zwei **NavigationProperty** -Elementen:</span><span class="sxs-lookup"><span data-stu-id="0f046-486">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="0f046-487">EnumType-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-487">EnumType Element (CSDL)</span></span>

<span data-ttu-id="0f046-488">Das **enumType** -Element stellt einen enumerierten Typ dar.</span><span class="sxs-lookup"><span data-stu-id="0f046-488">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="0f046-489">Ein **enumType** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="0f046-489">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f046-490">Dokumentation (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-490">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f046-491">Member (0 (null) oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-491">Member (zero or more elements)</span></span>
-   <span data-ttu-id="0f046-492">Annotation-Elemente (0 (null) oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-492">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-493">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-493">Applicable Attributes</span></span>

<span data-ttu-id="0f046-494">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **enumType** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-494">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="0f046-495">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-495">Attribute Name</span></span>     | <span data-ttu-id="0f046-496">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-496">Is Required</span></span> | <span data-ttu-id="0f046-497">value</span><span class="sxs-lookup"><span data-stu-id="0f046-497">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f046-498">**Name**</span><span class="sxs-lookup"><span data-stu-id="0f046-498">**Name**</span></span>           | <span data-ttu-id="0f046-499">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-499">Yes</span></span>         | <span data-ttu-id="0f046-500">Der Name des Entitätstyps.</span><span class="sxs-lookup"><span data-stu-id="0f046-500">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="0f046-501">**IsFlags**</span><span class="sxs-lookup"><span data-stu-id="0f046-501">**IsFlags**</span></span>        | <span data-ttu-id="0f046-502">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-502">No</span></span>          | <span data-ttu-id="0f046-503">**True** oder **false**, abhängig davon, ob der Enumeration-Typ als Satz von Flags verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="0f046-503">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="0f046-504">Der Standardwert ist **false.**</span><span class="sxs-lookup"><span data-stu-id="0f046-504">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="0f046-505">**Underlyingtype**</span><span class="sxs-lookup"><span data-stu-id="0f046-505">**UnderlyingType**</span></span> | <span data-ttu-id="0f046-506">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-506">No</span></span>          | <span data-ttu-id="0f046-507">**Edm. Byte**, **Edm. Int16**, **Edm. Int32**, **Edm. Int64** oder **Edm. SByte** , das den Wertebereich des Typs definiert.</span><span class="sxs-lookup"><span data-stu-id="0f046-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span> <span data-ttu-id="0f046-508">  Der Standard zugrunde liegende Typ der Enumerationselemente ist **Edm. Int32.** .</span><span class="sxs-lookup"><span data-stu-id="0f046-508">  The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f046-509">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **enumType** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-509">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="0f046-510">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-510">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-511">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-511">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f046-512">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-512">Example</span></span>

<span data-ttu-id="0f046-513">Das folgende Beispiel zeigt ein **enumType** -Element mit drei **Member** -Elementen:</span><span class="sxs-lookup"><span data-stu-id="0f046-513">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="0f046-514">Function-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-514">Function Element (CSDL)</span></span>

<span data-ttu-id="0f046-515">Das **Function** -Element in der konzeptionellen Schema Definitions Sprache (CSDL) wird verwendet, um Funktionen im konzeptionellen Modell zu definieren oder zu deklarieren.</span><span class="sxs-lookup"><span data-stu-id="0f046-515">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="0f046-516">Eine Funktion wird mit einem DefiningExpression-Element definiert.</span><span class="sxs-lookup"><span data-stu-id="0f046-516">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="0f046-517">Ein **Function** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="0f046-517">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f046-518">Dokumentation (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-518">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f046-519">Parameter (kein Element (null) oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-519">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="0f046-520">DefiningExpression (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-520">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="0f046-521">ReturnType (Funktion) (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-521">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="0f046-522">Annotation-Elemente (0 (null) oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-522">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="0f046-523">Ein Rückgabetyp für eine Funktion muss entweder mit dem **returnType** -Element (Function) oder dem **returnType** -Attribut angegeben werden (siehe unten), aber nicht mit beiden.</span><span class="sxs-lookup"><span data-stu-id="0f046-523">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="0f046-524">Die möglichen Rückgabetypen sind alle EdmSimpleType-Typen, Entitätstypen, komplexe Typen, Zeilentypen (oder eine Auflistung eines dieser Typen) oder Ref-Typen.</span><span class="sxs-lookup"><span data-stu-id="0f046-524">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-525">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-525">Applicable Attributes</span></span>

<span data-ttu-id="0f046-526">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **Function** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-526">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="0f046-527">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-527">Attribute Name</span></span> | <span data-ttu-id="0f046-528">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-528">Is Required</span></span> | <span data-ttu-id="0f046-529">value</span><span class="sxs-lookup"><span data-stu-id="0f046-529">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="0f046-530">**Name**</span><span class="sxs-lookup"><span data-stu-id="0f046-530">**Name**</span></span>       | <span data-ttu-id="0f046-531">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-531">Yes</span></span>         | <span data-ttu-id="0f046-532">Der Name der Funktion.</span><span class="sxs-lookup"><span data-stu-id="0f046-532">The name of the function.</span></span>          |
| <span data-ttu-id="0f046-533">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="0f046-533">**ReturnType**</span></span> | <span data-ttu-id="0f046-534">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-534">No</span></span>          | <span data-ttu-id="0f046-535">Der von der Funktion zurückgegebene Typ.</span><span class="sxs-lookup"><span data-stu-id="0f046-535">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f046-536">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **Function** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-536">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="0f046-537">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-537">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-538">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-538">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f046-539">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-539">Example</span></span>

<span data-ttu-id="0f046-540">Im folgenden Beispiel wird ein **Function** -Element verwendet, um eine Funktion zu definieren, die die Anzahl der Jahre zurückgibt, seit ein Dozenten eingestellt wurde.</span><span class="sxs-lookup"><span data-stu-id="0f046-540">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="0f046-541">FunctionImport-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-541">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="0f046-542">Das **FunctionImport** -Element in konzeptioneller Schema Definitions Sprache (CSDL) stellt eine Funktion dar, die in der Datenquelle definiert ist, aber für-Objekte über das konzeptionelle Modell verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-542">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="0f046-543">Ein Function-Element im Speichermodell kann z. B. verwendet werden, um eine gespeicherte Prozedur in einer Datenbank darzustellen.</span><span class="sxs-lookup"><span data-stu-id="0f046-543">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="0f046-544">Ein **FunctionImport** -Element im konzeptionellen Modell stellt die entsprechende Funktion in einer Entity Framework Anwendung dar und wird mithilfe des FunctionImportMapping-Elements der Speichermodell Funktion zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="0f046-544">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="0f046-545">Wird die Funktion in der Anwendung aufgerufen, wird die entsprechende gespeicherte Prozedur in der Datenbank ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="0f046-545">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="0f046-546">Das **FunctionImport** -Element kann über die folgenden untergeordneten Elemente verfügen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="0f046-546">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f046-547">Dokumentation (kein (null) oder ein Element zulässig)</span><span class="sxs-lookup"><span data-stu-id="0f046-547">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="0f046-548">Parameter (kein Element (null) oder mehrere Elemente zugelassen)</span><span class="sxs-lookup"><span data-stu-id="0f046-548">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="0f046-549">Annotation-Elemente (0 (null) oder mehr Elemente zulässig)</span><span class="sxs-lookup"><span data-stu-id="0f046-549">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="0f046-550">ReturnType (FunctionImport) (0 (null) oder mehr Elemente zulässig)</span><span class="sxs-lookup"><span data-stu-id="0f046-550">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="0f046-551">Für jeden Parameter, den die Funktion akzeptiert, muss ein **Parameter** Element definiert werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-551">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="0f046-552">Ein Rückgabetyp für eine Funktion muss entweder mit dem **returnType** -Element (FunctionImport) oder dem **returnType** -Attribut angegeben werden (siehe unten), aber nicht mit beiden.</span><span class="sxs-lookup"><span data-stu-id="0f046-552">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="0f046-553">Der Rückgabetyp Wert muss eine Auflistung von edmsimpletype, EntityType oder complexType sein.</span><span class="sxs-lookup"><span data-stu-id="0f046-553">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-554">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-554">Applicable Attributes</span></span>

<span data-ttu-id="0f046-555">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **FunctionImport** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-555">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="0f046-556">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-556">Attribute Name</span></span>   | <span data-ttu-id="0f046-557">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-557">Is Required</span></span> | <span data-ttu-id="0f046-558">value</span><span class="sxs-lookup"><span data-stu-id="0f046-558">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f046-559">**Name**</span><span class="sxs-lookup"><span data-stu-id="0f046-559">**Name**</span></span>         | <span data-ttu-id="0f046-560">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-560">Yes</span></span>         | <span data-ttu-id="0f046-561">Der Name der importierten Funktion.</span><span class="sxs-lookup"><span data-stu-id="0f046-561">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="0f046-562">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="0f046-562">**ReturnType**</span></span>   | <span data-ttu-id="0f046-563">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-563">No</span></span>          | <span data-ttu-id="0f046-564">Der Typ, den die Funktion zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="0f046-564">The type that the function returns.</span></span> <span data-ttu-id="0f046-565">Verwenden Sie dieses Attribut nicht, wenn die Funktion keinen Wert zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="0f046-565">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="0f046-566">Andernfalls muss der Wert eine Auflistung von complexType, EntityType oder edmsimpletype sein.</span><span class="sxs-lookup"><span data-stu-id="0f046-566">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="0f046-567">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="0f046-567">**EntitySet**</span></span>    | <span data-ttu-id="0f046-568">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-568">No</span></span>          | <span data-ttu-id="0f046-569">Wenn die Funktion eine Auflistung von Entitäts Typen zurückgibt, muss der Wert von **EntitySet** der Entitätenmenge angehören, zu der die Auflistung gehört.</span><span class="sxs-lookup"><span data-stu-id="0f046-569">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="0f046-570">Andernfalls darf das Attribut **EntitySet** nicht verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-570">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="0f046-571">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="0f046-571">**IsComposable**</span></span> | <span data-ttu-id="0f046-572">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-572">No</span></span>          | <span data-ttu-id="0f046-573">Wenn der Wert auf true festgelegt ist, ist die Funktion zusammensetzbar (Tabellenwert Funktion) und kann in einer LINQ-Abfrage verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-573">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span><span data-ttu-id="0f046-574">  Der Standardwert ist **false**.</span><span class="sxs-lookup"><span data-stu-id="0f046-574">  The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="0f046-575">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **FunctionImport** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-575">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="0f046-576">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-576">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-577">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-577">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f046-578">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-578">Example</span></span>

<span data-ttu-id="0f046-579">Das folgende Beispiel zeigt ein **FunctionImport** -Element, das einen Parameter akzeptiert und eine Auflistung von Entitäts Typen zurückgibt:</span><span class="sxs-lookup"><span data-stu-id="0f046-579">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="0f046-580">Key-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-580">Key Element (CSDL)</span></span>

<span data-ttu-id="0f046-581">Das **Key** -Element ist ein untergeordnetes Element des EntityType-Elements und definiert einen *Entitäts Schlüssel* (eine Eigenschaft oder einen Satz von Eigenschaften eines Entitäts Typs, der die Identität bestimmt).</span><span class="sxs-lookup"><span data-stu-id="0f046-581">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="0f046-582">Die Eigenschaften, die einen Entitätsschlüssel bilden, werden zur Entwurfszeit ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="0f046-582">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="0f046-583">Die Werte von Entitätsschlüsseleigenschaften müssen zur Laufzeit eindeutig eine Entitätstypinstanz innerhalb einer Entitätenmenge identifizieren.</span><span class="sxs-lookup"><span data-stu-id="0f046-583">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="0f046-584">Die Eigenschaften, die einen Entitätsschlüssel bilden, sollten so ausgewählt werden, dass die Eindeutigkeit von Instanzen in einem Entitätssatz gewährleistet ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-584">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="0f046-585">Das **Key** -Element definiert einen Entitäts Schlüssel, indem auf eine oder mehrere Eigenschaften eines Entitäts Typs verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-585">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="0f046-586">Das **Key** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="0f046-586">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="0f046-587">PropertyRef (ein oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-587">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="0f046-588">Annotation-Elemente (0 (null) oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-588">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-589">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-589">Applicable Attributes</span></span>

<span data-ttu-id="0f046-590">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **Schlüssel** Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-590">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="0f046-591">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-592">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="0f046-593">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-593">Example</span></span>

<span data-ttu-id="0f046-594">Im folgenden Beispiel wird ein Entitätstyp mit dem Namen **Book**definiert.</span><span class="sxs-lookup"><span data-stu-id="0f046-594">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="0f046-595">Der Entitäts Schlüssel wird definiert, indem auf die **ISBN** -Eigenschaft des Entitäts Typs verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-595">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

<span data-ttu-id="0f046-596">Die **ISBN** -Eigenschaft ist eine gute Wahl für den Entitäts Schlüssel, weil eine internationale Standard Buchnummer (ISBN) ein Buch eindeutig identifiziert.</span><span class="sxs-lookup"><span data-stu-id="0f046-596">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="0f046-597">Das folgende Beispiel zeigt einen Entitätstyp (**Author**), der über einen Entitäts Schlüssel verfügt, der aus zwei Eigenschaften, **Name** und **Adresse**besteht.</span><span class="sxs-lookup"><span data-stu-id="0f046-597">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

``` xml
 <EntityType Name="Author">
   <Key>
     <PropertyRef Name="Name" />
     <PropertyRef Name="Address" />
   </Key>
   <Property Type="String" Name="Name" Nullable="false" />
   <Property Type="String" Name="Address" Nullable="false" />
   <NavigationProperty Name="Books" Relationship="BooksModel.WrittenBy"
                       FromRole="Author" ToRole="Book" />
 </EntityType>
```
 

<span data-ttu-id="0f046-598">Die Verwendung von **Name** und **Adresse** für den Entitäts Schlüssel ist eine sinnvolle Wahl, da es unwahrscheinlich ist, dass sich zwei Autoren mit demselben Namen an derselben Adresse befinden.</span><span class="sxs-lookup"><span data-stu-id="0f046-598">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="0f046-599">Dieser Entitätsschlüssel garantiert jedoch nicht absolut eindeutige Entitätsschlüssel in einem Entitätssatz.</span><span class="sxs-lookup"><span data-stu-id="0f046-599">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="0f046-600">In diesem Fall empfiehlt es sich, eine Eigenschaft hinzuzufügen, z. **b. Autorität**, die zur eindeutigen Identifizierung eines Autors verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="0f046-600">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="0f046-601">Member-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-601">Member Element (CSDL)</span></span>

<span data-ttu-id="0f046-602">Das **Member** -Element ist ein untergeordnetes Element des enumType-Elements und definiert einen Member des enumerierten Typs.</span><span class="sxs-lookup"><span data-stu-id="0f046-602">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-603">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-603">Applicable Attributes</span></span>

<span data-ttu-id="0f046-604">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **FunctionImport** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-604">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="0f046-605">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-605">Attribute Name</span></span> | <span data-ttu-id="0f046-606">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-606">Is Required</span></span> | <span data-ttu-id="0f046-607">value</span><span class="sxs-lookup"><span data-stu-id="0f046-607">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f046-608">**Name**</span><span class="sxs-lookup"><span data-stu-id="0f046-608">**Name**</span></span>       | <span data-ttu-id="0f046-609">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-609">Yes</span></span>         | <span data-ttu-id="0f046-610">Der Name des Elements.</span><span class="sxs-lookup"><span data-stu-id="0f046-610">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="0f046-611">**Wert**</span><span class="sxs-lookup"><span data-stu-id="0f046-611">**Value**</span></span>      | <span data-ttu-id="0f046-612">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-612">No</span></span>          | <span data-ttu-id="0f046-613">Der Wert des Members.</span><span class="sxs-lookup"><span data-stu-id="0f046-613">The value of the member.</span></span> <span data-ttu-id="0f046-614">Standardmäßig hat der erste Member den Wert 0, und der Wert jedes nachfolgenden Enumerators wird um 1 erhöht.</span><span class="sxs-lookup"><span data-stu-id="0f046-614">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="0f046-615">Es können mehrere Member mit denselben Werten vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="0f046-615">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f046-616">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **FunctionImport** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-616">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="0f046-617">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-617">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-618">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-618">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f046-619">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-619">Example</span></span>

<span data-ttu-id="0f046-620">Das folgende Beispiel zeigt ein **enumType** -Element mit drei **Member** -Elementen:</span><span class="sxs-lookup"><span data-stu-id="0f046-620">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="0f046-621">NavigationProperty-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-621">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="0f046-622">Ein **NavigationProperty** -Element definiert eine Navigations Eigenschaft, die einen Verweis auf das andere Ende einer Zuordnung bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="0f046-622">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="0f046-623">Im Gegensatz zu mit dem Property-Element definierten Eigenschaften werden von Navigationseigenschaften Form und Eigenschaften von Daten nicht definiert.</span><span class="sxs-lookup"><span data-stu-id="0f046-623">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="0f046-624">Sie bieten eine Möglichkeit, eine Zuordnung zwischen zwei Entitätstypen zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="0f046-624">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="0f046-625">Beachten Sie, dass Navigationseigenschaften für beide Entitätstypen an den Enden einer Zuordnung optional sind.</span><span class="sxs-lookup"><span data-stu-id="0f046-625">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="0f046-626">Wenn Sie für einen Entitätstyp am Ende einer Zuordnung eine Navigationseigenschaft definieren, muss keine Navigationseigenschaft für den Entitätstyp am anderen Ende der Zuordnung definiert werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-626">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="0f046-627">Der von einer Navigationseigenschaft zurückgegebene Datentyp wird von der Multiplizität des Remotezuordnungsendes bestimmt.</span><span class="sxs-lookup"><span data-stu-id="0f046-627">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="0f046-628">Angenommen, eine Navigations Eigenschaft, **ordersnavprop**, ist für einen **Customer** -Entitätstyp vorhanden und navigiert zu einer 1: n-Zuordnung zwischen **Customer** und **Order**.</span><span class="sxs-lookup"><span data-stu-id="0f046-628">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="0f046-629">Da das Remote Zuordnungs Ende für die Navigations Eigenschaft eine Multiplizität many (\*) aufweist, ist sein Datentyp eine Auflistung (der **Reihenfolge**).</span><span class="sxs-lookup"><span data-stu-id="0f046-629">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="0f046-630">Wenn eine Navigations Eigenschaft, **customernavprop**, auf dem **Order** -Entitätstyp vorhanden ist, wäre der Datentyp " **Customer** ", da die Multiplizität des Remote Endes "eins" (1) ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-630">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="0f046-631">Ein **NavigationProperty** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="0f046-631">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f046-632">Dokumentation (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-632">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f046-633">Annotation-Elemente (0 (null) oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-633">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-634">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-634">Applicable Attributes</span></span>

<span data-ttu-id="0f046-635">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **NavigationProperty** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-635">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="0f046-636">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-636">Attribute Name</span></span>   | <span data-ttu-id="0f046-637">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-637">Is Required</span></span> | <span data-ttu-id="0f046-638">value</span><span class="sxs-lookup"><span data-stu-id="0f046-638">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f046-639">**Name**</span><span class="sxs-lookup"><span data-stu-id="0f046-639">**Name**</span></span>         | <span data-ttu-id="0f046-640">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-640">Yes</span></span>         | <span data-ttu-id="0f046-641">Der Name der Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="0f046-641">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="0f046-642">**Verhältnis**</span><span class="sxs-lookup"><span data-stu-id="0f046-642">**Relationship**</span></span> | <span data-ttu-id="0f046-643">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-643">Yes</span></span>         | <span data-ttu-id="0f046-644">Der Name einer Zuordnung, die sich innerhalb des Bereichs des Modells befindet.</span><span class="sxs-lookup"><span data-stu-id="0f046-644">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="0f046-645">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="0f046-645">**ToRole**</span></span>       | <span data-ttu-id="0f046-646">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-646">Yes</span></span>         | <span data-ttu-id="0f046-647">Das Ende der Zuordnung, an dem die Navigation endet.</span><span class="sxs-lookup"><span data-stu-id="0f046-647">The end of the association at which navigation ends.</span></span> <span data-ttu-id="0f046-648">Der Wert des Attributs " **Tor** " muss mit dem Wert eines der **Rollen** Attribute übereinstimmen, die für eine der Zuordnungs enden definiert sind (definiert im AssociationEnd-Element).</span><span class="sxs-lookup"><span data-stu-id="0f046-648">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="0f046-649">**FromRole**</span><span class="sxs-lookup"><span data-stu-id="0f046-649">**FromRole**</span></span>     | <span data-ttu-id="0f046-650">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-650">Yes</span></span>         | <span data-ttu-id="0f046-651">Das Ende der Zuordnung, an dem die Navigation beginnt.</span><span class="sxs-lookup"><span data-stu-id="0f046-651">The end of the association from which navigation begins.</span></span> <span data-ttu-id="0f046-652">Der Wert des **FromRole** -Attributs muss mit dem Wert eines der **Rollen** Attribute übereinstimmen, die für eine der Zuordnungs enden definiert sind (definiert im AssociationEnd-Element).</span><span class="sxs-lookup"><span data-stu-id="0f046-652">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f046-653">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **NavigationProperty** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-653">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="0f046-654">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-654">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-655">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-655">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f046-656">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-656">Example</span></span>

<span data-ttu-id="0f046-657">Im folgenden Beispiel wird ein Entitätstyp (**Book**) mit zwei Navigations Eigenschaften (**PublishedBy** und **Schreibweise**) definiert:</span><span class="sxs-lookup"><span data-stu-id="0f046-657">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="0f046-658">OnDelete-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-658">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="0f046-659">Das **OnDelete** -Element in konzeptioneller Schema Definitions Sprache (CSDL) definiert das Verhalten, das mit einer Zuordnung verbunden ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-659">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="0f046-660">Wenn das **Action** -Attribut an einem Ende einer Zuordnung auf **Cascade** festgelegt ist, werden verwandte Entitäts Typen am anderen Ende der Zuordnung gelöscht, wenn der Entitätstyp am ersten Ende gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-660">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="0f046-661">Wenn die Zuordnung zwischen zwei Entitäts Typen eine Primärschlüssel-zu-Primärschlüssel-Beziehung ist, wird ein geladenes abhängiges Objekt gelöscht, wenn das Prinzipal Objekt am anderen Ende der Zuordnung unabhängig von der **OnDelete** -Spezifikation gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-661">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="0f046-662">Das **OnDelete** -Element wirkt sich nur auf das Laufzeitverhalten einer Anwendung aus. Dies wirkt sich nicht auf das Verhalten in der Datenquelle aus.</span><span class="sxs-lookup"><span data-stu-id="0f046-662">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="0f046-663">Das in der Datenquelle definierte Verhalten sollte dem in der Anwendung definierten Verhalten entsprechen.</span><span class="sxs-lookup"><span data-stu-id="0f046-663">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="0f046-664">Ein **OnDelete** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="0f046-664">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f046-665">Dokumentation (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-665">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f046-666">Annotation-Elemente (0 (null) oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-666">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-667">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-667">Applicable Attributes</span></span>

<span data-ttu-id="0f046-668">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **OnDelete** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-668">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="0f046-669">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-669">Attribute Name</span></span> | <span data-ttu-id="0f046-670">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-670">Is Required</span></span> | <span data-ttu-id="0f046-671">value</span><span class="sxs-lookup"><span data-stu-id="0f046-671">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f046-672">**Aktion**</span><span class="sxs-lookup"><span data-stu-id="0f046-672">**Action**</span></span>     | <span data-ttu-id="0f046-673">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-673">Yes</span></span>         | <span data-ttu-id="0f046-674">**Cascade** oder **None**.</span><span class="sxs-lookup"><span data-stu-id="0f046-674">**Cascade** or **None**.</span></span> <span data-ttu-id="0f046-675">Wenn **Cascade**verwendet wird, werden abhängige Entitäts Typen gelöscht, wenn der Prinzipal Entitätstyp gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-675">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="0f046-676">Wenn **keine**, werden abhängige Entitäts Typen nicht gelöscht, wenn der Prinzipal Entitätstyp gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-676">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f046-677">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **Association** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-677">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="0f046-678">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-678">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-679">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-679">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f046-680">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-680">Example</span></span>

<span data-ttu-id="0f046-681">Das folgende Beispiel zeigt ein **Association** -Element, das die Zuordnung **CustomerOrders** definiert.</span><span class="sxs-lookup"><span data-stu-id="0f046-681">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="0f046-682">Das **OnDelete** -Element gibt an, dass alle **Bestellungen** , die mit einem bestimmten **Kunden** verknüpft sind und in den ObjectContext geladen wurden, gelöscht werden, wenn der **Kunde** gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-682">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="0f046-683">Parameter-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-683">Parameter Element (CSDL)</span></span>

<span data-ttu-id="0f046-684">Das **Parameter** -Element in konzeptioneller Schema Definitions Sprache (CSDL) kann ein untergeordnetes Element des FunctionImport-Elements oder des Function-Elements sein.</span><span class="sxs-lookup"><span data-stu-id="0f046-684">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="0f046-685">FunctionImport-Element-Anwendung</span><span class="sxs-lookup"><span data-stu-id="0f046-685">FunctionImport Element Application</span></span>

<span data-ttu-id="0f046-686">Ein **Parameter** Element (als untergeordnetes Element des **FunctionImport** -Elements) wird verwendet, um Eingabe-und Ausgabeparameter für Funktions Importe zu definieren, die in CSDL deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-686">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="0f046-687">Das **Parameter** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="0f046-687">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f046-688">Dokumentation (kein (null) oder ein Element zulässig)</span><span class="sxs-lookup"><span data-stu-id="0f046-688">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="0f046-689">Annotation-Elemente (0 (null) oder mehr Elemente zulässig)</span><span class="sxs-lookup"><span data-stu-id="0f046-689">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="0f046-690">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-690">Applicable Attributes</span></span>

<span data-ttu-id="0f046-691">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **Parameter** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-691">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="0f046-692">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-692">Attribute Name</span></span> | <span data-ttu-id="0f046-693">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-693">Is Required</span></span> | <span data-ttu-id="0f046-694">value</span><span class="sxs-lookup"><span data-stu-id="0f046-694">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f046-695">**Name**</span><span class="sxs-lookup"><span data-stu-id="0f046-695">**Name**</span></span>       | <span data-ttu-id="0f046-696">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-696">Yes</span></span>         | <span data-ttu-id="0f046-697">Der Name des Parameters.</span><span class="sxs-lookup"><span data-stu-id="0f046-697">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="0f046-698">**Typ**</span><span class="sxs-lookup"><span data-stu-id="0f046-698">**Type**</span></span>       | <span data-ttu-id="0f046-699">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-699">Yes</span></span>         | <span data-ttu-id="0f046-700">Der Parametertyp.</span><span class="sxs-lookup"><span data-stu-id="0f046-700">The parameter type.</span></span> <span data-ttu-id="0f046-701">Der Wert muss ein **EDMSimpleType** oder komplexer Typ sein, der im Gültigkeitsbereich des Modells liegt.</span><span class="sxs-lookup"><span data-stu-id="0f046-701">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="0f046-702">**Mode**</span><span class="sxs-lookup"><span data-stu-id="0f046-702">**Mode**</span></span>       | <span data-ttu-id="0f046-703">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-703">No</span></span>          | <span data-ttu-id="0f046-704">**In**, out oder **INOUT** , je nachdem, ob der Parameter ein Eingabe-, Ausgabe-oder Eingabe- **/Ausgabeparameter**ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-704">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="0f046-705">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="0f046-705">**MaxLength**</span></span>  | <span data-ttu-id="0f046-706">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-706">No</span></span>          | <span data-ttu-id="0f046-707">Die maximal zulässige Länge des Parameters.</span><span class="sxs-lookup"><span data-stu-id="0f046-707">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="0f046-708">**Genauigkeit**</span><span class="sxs-lookup"><span data-stu-id="0f046-708">**Precision**</span></span>  | <span data-ttu-id="0f046-709">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-709">No</span></span>          | <span data-ttu-id="0f046-710">Die Genauigkeit des Parameters.</span><span class="sxs-lookup"><span data-stu-id="0f046-710">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="0f046-711">**Skalieren**</span><span class="sxs-lookup"><span data-stu-id="0f046-711">**Scale**</span></span>      | <span data-ttu-id="0f046-712">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-712">No</span></span>          | <span data-ttu-id="0f046-713">Der Maßstab des Parameters.</span><span class="sxs-lookup"><span data-stu-id="0f046-713">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="0f046-714">**SRID**</span><span class="sxs-lookup"><span data-stu-id="0f046-714">**SRID**</span></span>       | <span data-ttu-id="0f046-715">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-715">No</span></span>          | <span data-ttu-id="0f046-716">Verweis Bezeichner für räumliche Systeme.</span><span class="sxs-lookup"><span data-stu-id="0f046-716">Spatial System Reference Identifier.</span></span> <span data-ttu-id="0f046-717">Nur für Parameter räumlicher Typen gültig.</span><span class="sxs-lookup"><span data-stu-id="0f046-717">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="0f046-718">Weitere Informationen finden Sie unter [SRID](https://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="0f046-718">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f046-719">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **Parameter** Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-719">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="0f046-720">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-720">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-721">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-721">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="0f046-722">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-722">Example</span></span>

<span data-ttu-id="0f046-723">Das folgende Beispiel zeigt ein **FunctionImport** -Element mit einem untergeordneten **Parameter** -Element.</span><span class="sxs-lookup"><span data-stu-id="0f046-723">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="0f046-724">Die Funktion akzeptiert einen Eingabeparameter und gibt eine Auflistung von Entitätstypen zurück.</span><span class="sxs-lookup"><span data-stu-id="0f046-724">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="0f046-725">Function-Element-Anwendung</span><span class="sxs-lookup"><span data-stu-id="0f046-725">Function Element Application</span></span>

<span data-ttu-id="0f046-726">Ein **Parameter** Element (als untergeordnetes Element des **Function** -Elements) definiert Parameter für Funktionen, die in einem konzeptionellen Modell definiert oder deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-726">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="0f046-727">Das **Parameter** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="0f046-727">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f046-728">Dokumentation (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-728">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="0f046-729">CollectionType (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-729">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="0f046-730">ReferenceType (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-730">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="0f046-731">RowType (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-731">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="0f046-732">Nur eines der **CollectionType**-, **ReferenceType**-oder **RowType** -Elemente kann ein untergeordnetes Element eines **Property** -Elements sein.</span><span class="sxs-lookup"><span data-stu-id="0f046-732">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="0f046-733">Annotation-Elemente (0 (null) oder mehr Elemente zulässig)</span><span class="sxs-lookup"><span data-stu-id="0f046-733">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="0f046-734">Anmerkungselemente müssen an alle anderen untergeordneten Elemente angereiht werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-734">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="0f046-735">Annotation-Elemente sind nur in CSDL v2 und höher zulässig.</span><span class="sxs-lookup"><span data-stu-id="0f046-735">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="0f046-736">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-736">Applicable Attributes</span></span>

<span data-ttu-id="0f046-737">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **Parameter** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-737">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="0f046-738">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-738">Attribute Name</span></span>   | <span data-ttu-id="0f046-739">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-739">Is Required</span></span> | <span data-ttu-id="0f046-740">value</span><span class="sxs-lookup"><span data-stu-id="0f046-740">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f046-741">**Name**</span><span class="sxs-lookup"><span data-stu-id="0f046-741">**Name**</span></span>         | <span data-ttu-id="0f046-742">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-742">Yes</span></span>         | <span data-ttu-id="0f046-743">Der Name des Parameters.</span><span class="sxs-lookup"><span data-stu-id="0f046-743">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="0f046-744">**Typ**</span><span class="sxs-lookup"><span data-stu-id="0f046-744">**Type**</span></span>         | <span data-ttu-id="0f046-745">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-745">No</span></span>          | <span data-ttu-id="0f046-746">Der Parametertyp.</span><span class="sxs-lookup"><span data-stu-id="0f046-746">The parameter type.</span></span> <span data-ttu-id="0f046-747">Ein Parameter kann einer der folgenden Typen (oder Auflistungen dieser Typen) sein:</span><span class="sxs-lookup"><span data-stu-id="0f046-747">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="0f046-748">**EdmSimpleType**</span><span class="sxs-lookup"><span data-stu-id="0f046-748">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="0f046-749">Entitätstyp</span><span class="sxs-lookup"><span data-stu-id="0f046-749">entity type</span></span> <br/> <span data-ttu-id="0f046-750">Komplexer Typ</span><span class="sxs-lookup"><span data-stu-id="0f046-750">complex type</span></span> <br/> <span data-ttu-id="0f046-751">Zeilentyp</span><span class="sxs-lookup"><span data-stu-id="0f046-751">row type</span></span> <br/> <span data-ttu-id="0f046-752">Verweistyp</span><span class="sxs-lookup"><span data-stu-id="0f046-752">reference type</span></span>                             |
| <span data-ttu-id="0f046-753">**NULL zulassen**</span><span class="sxs-lookup"><span data-stu-id="0f046-753">**Nullable**</span></span>     | <span data-ttu-id="0f046-754">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-754">No</span></span>          | <span data-ttu-id="0f046-755">**True** (Standardwert) oder **false** , abhängig davon, ob die Eigenschaft einen **null** -Wert aufweisen kann.</span><span class="sxs-lookup"><span data-stu-id="0f046-755">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="0f046-756">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="0f046-756">**DefaultValue**</span></span> | <span data-ttu-id="0f046-757">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-757">No</span></span>          | <span data-ttu-id="0f046-758">Der Standardwert der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="0f046-758">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="0f046-759">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="0f046-759">**MaxLength**</span></span>    | <span data-ttu-id="0f046-760">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-760">No</span></span>          | <span data-ttu-id="0f046-761">Maximale Länge des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="0f046-761">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="0f046-762">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="0f046-762">**FixedLength**</span></span>  | <span data-ttu-id="0f046-763">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-763">No</span></span>          | <span data-ttu-id="0f046-764">**True** oder **false** , abhängig davon, ob der Eigenschafts Wert als Zeichenfolge mit fester Länge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-764">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="0f046-765">**Genauigkeit**</span><span class="sxs-lookup"><span data-stu-id="0f046-765">**Precision**</span></span>    | <span data-ttu-id="0f046-766">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-766">No</span></span>          | <span data-ttu-id="0f046-767">Die Genauigkeit des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="0f046-767">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="0f046-768">**Skalieren**</span><span class="sxs-lookup"><span data-stu-id="0f046-768">**Scale**</span></span>        | <span data-ttu-id="0f046-769">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-769">No</span></span>          | <span data-ttu-id="0f046-770">Die Skalierung des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="0f046-770">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="0f046-771">**SRID**</span><span class="sxs-lookup"><span data-stu-id="0f046-771">**SRID**</span></span>         | <span data-ttu-id="0f046-772">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-772">No</span></span>          | <span data-ttu-id="0f046-773">Verweis Bezeichner für räumliche Systeme.</span><span class="sxs-lookup"><span data-stu-id="0f046-773">Spatial System Reference Identifier.</span></span> <span data-ttu-id="0f046-774">Nur für Eigenschaften räumlicher Typen gültig.</span><span class="sxs-lookup"><span data-stu-id="0f046-774">Valid only for properties of spatial types.</span></span> <span data-ttu-id="0f046-775">Weitere Informationen finden Sie unter [SRID](https://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="0f046-775">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="0f046-776">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="0f046-776">**Unicode**</span></span>      | <span data-ttu-id="0f046-777">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-777">No</span></span>          | <span data-ttu-id="0f046-778">**True** oder **false** , abhängig davon, ob der Eigenschafts Wert als Unicode-Zeichenfolge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-778">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="0f046-779">**Sortierung**</span><span class="sxs-lookup"><span data-stu-id="0f046-779">**Collation**</span></span>    | <span data-ttu-id="0f046-780">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-780">No</span></span>          | <span data-ttu-id="0f046-781">Eine Zeichenfolge, die angibt, welche Sortierreihenfolge in der Datenquelle verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-781">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="0f046-782">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **Parameter** Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-782">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="0f046-783">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-783">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-784">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-784">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="0f046-785">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-785">Example</span></span>

<span data-ttu-id="0f046-786">Das folgende Beispiel zeigt ein **Function** -Element, das ein untergeordnetes **Parameter** Element verwendet, um einen Funktions Parameter zu definieren.</span><span class="sxs-lookup"><span data-stu-id="0f046-786">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a><span data-ttu-id="0f046-787">Principal-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-787">Principal Element (CSDL)</span></span>

<span data-ttu-id="0f046-788">Das **Principal** -Element in konzeptioneller Schema Definitions Sprache (CSDL) ist ein untergeordnetes Element für das referentialeinschränkung-Element, das das Prinzipal Ende einer referenziellen Einschränkung definiert.</span><span class="sxs-lookup"><span data-stu-id="0f046-788">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="0f046-789">Ein **referentialeinschränkung** -Element definiert Funktionen, die einer Einschränkung der referenziellen Integrität in einer relationalen Datenbank ähneln.</span><span class="sxs-lookup"><span data-stu-id="0f046-789">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="0f046-790">So wie eine Spalte (oder Spalten) einer Datenbanktabelle auf den Primärschlüssel einer anderen Tabelle verweisen kann, kann eine Eigenschaft (oder Eigenschaften) eines Entitätstyps auf den Entitätsschlüssel eines anderen Entitätstyps verweisen.</span><span class="sxs-lookup"><span data-stu-id="0f046-790">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="0f046-791">Der Entitätstyp, auf den verwiesen wird, wird als *Prinzipal Ende* der Einschränkung bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="0f046-791">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="0f046-792">Der Entitätstyp, der auf das Prinzipal Ende verweist, wird als *abhängiges Ende* der Einschränkung bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="0f046-792">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="0f046-793">**PropertyRef** -Elemente werden verwendet, um anzugeben, auf welche Schlüssel vom abhängigen Ende verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-793">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="0f046-794">Das **Principal** -Element kann über die folgenden untergeordneten Elemente verfügen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="0f046-794">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f046-795">PropertyRef (ein oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-795">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="0f046-796">Annotation-Elemente (0 (null) oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-796">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-797">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-797">Applicable Attributes</span></span>

<span data-ttu-id="0f046-798">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **Principal** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-798">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="0f046-799">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-799">Attribute Name</span></span> | <span data-ttu-id="0f046-800">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-800">Is Required</span></span> | <span data-ttu-id="0f046-801">value</span><span class="sxs-lookup"><span data-stu-id="0f046-801">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="0f046-802">**Rolle**</span><span class="sxs-lookup"><span data-stu-id="0f046-802">**Role**</span></span>       | <span data-ttu-id="0f046-803">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-803">Yes</span></span>         | <span data-ttu-id="0f046-804">Der Name des Entitätstyps am Prinzipalende der Zuordnung.</span><span class="sxs-lookup"><span data-stu-id="0f046-804">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f046-805">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **Principal** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-805">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="0f046-806">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-806">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-807">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-807">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f046-808">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-808">Example</span></span>

<span data-ttu-id="0f046-809">Das folgende Beispiel zeigt ein **referentialeinschränkungs** -Element, das Teil der Definition der **PublishedBy** -Zuordnung ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-809">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="0f046-810">Die **ID** -Eigenschaft des **Verleger** Entitäts Typs bildet das Prinzipal Ende der referenziellen Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="0f046-810">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="0f046-811">Property-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-811">Property Element (CSDL)</span></span>

<span data-ttu-id="0f046-812">Das **Property** -Element in konzeptioneller Schema Definitions Sprache (CSDL) kann ein untergeordnetes Element des EntityType-Elements, das complexType-Element oder das RowType-Element sein.</span><span class="sxs-lookup"><span data-stu-id="0f046-812">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="0f046-813">EntityType- und ComplexType-Element-Anwendungen</span><span class="sxs-lookup"><span data-stu-id="0f046-813">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="0f046-814">Eigenschaften Elemente (als unter **geordnete Elemente von** **EntityType** -oder **complexType** -Elementen) definieren die Form und die Eigenschaften der Daten, die eine Entitätstyp Instanz oder eine komplexe Typinstanz enthalten wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-814">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="0f046-815">Eigenschaften in einem konzeptionellen Modell sind analog zu den Eigenschaften, die für eine Klasse definiert werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-815">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="0f046-816">So wie Eigenschaften die Form einer Klasse definieren und Informationen zu Objekten enthalten definieren Eigenschaften in einem konzeptionellen Modell die Form eines Entitätstyps und enthalten Informationen zu Entitätstypinstanzen.</span><span class="sxs-lookup"><span data-stu-id="0f046-816">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="0f046-817">Das **Property** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="0f046-817">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f046-818">Documentation-Element (kein (null) oder ein Element zulässig)</span><span class="sxs-lookup"><span data-stu-id="0f046-818">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="0f046-819">Annotation-Elemente (0 (null) oder mehr Elemente zulässig)</span><span class="sxs-lookup"><span data-stu-id="0f046-819">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="0f046-820">Die folgenden Facetten können auf ein **Property** -Element angewendet werden **: Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **COLLATIONS**,-zustandcymode.</span><span class="sxs-lookup"><span data-stu-id="0f046-820">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="0f046-821">Facets sind XML-Attribute, die Informationen über die Speicherung von Eigenschaftswerten im Datenspeicher bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="0f046-821">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="0f046-822">Facetten können nur auf Eigenschaften vom Typ " **edmsimpletype**" angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-822">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="0f046-823">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-823">Applicable Attributes</span></span>

<span data-ttu-id="0f046-824">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **Property** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-824">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="0f046-825">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-825">Attribute Name</span></span>                                                         | <span data-ttu-id="0f046-826">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-826">Is Required</span></span> | <span data-ttu-id="0f046-827">value</span><span class="sxs-lookup"><span data-stu-id="0f046-827">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f046-828">**Name**</span><span class="sxs-lookup"><span data-stu-id="0f046-828">**Name**</span></span>                                                               | <span data-ttu-id="0f046-829">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-829">Yes</span></span>         | <span data-ttu-id="0f046-830">Der Name der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="0f046-830">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="0f046-831">**Typ**</span><span class="sxs-lookup"><span data-stu-id="0f046-831">**Type**</span></span>                                                               | <span data-ttu-id="0f046-832">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-832">Yes</span></span>         | <span data-ttu-id="0f046-833">Der Typ des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="0f046-833">The type of the property value.</span></span> <span data-ttu-id="0f046-834">Der Typ des Eigenschaftswerts muss ein **EDMSimpleType** oder ein komplexer Typ sein (erkennbar am vollqualifizierten Namen), der im Gültigkeitsbereich des Modells liegt.</span><span class="sxs-lookup"><span data-stu-id="0f046-834">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="0f046-835">**NULL zulassen**</span><span class="sxs-lookup"><span data-stu-id="0f046-835">**Nullable**</span></span>                                                           | <span data-ttu-id="0f046-836">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-836">No</span></span>          | <span data-ttu-id="0f046-837">**True** (Standardwert) oder <strong>False</strong> abhängig davon, ob die Eigenschaft einen NULL-Wert haben kann.</span><span class="sxs-lookup"><span data-stu-id="0f046-837">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="0f046-838">> In CSDL v1 muss eine komplexe Typeigenschaft `Nullable="False"`haben.</span><span class="sxs-lookup"><span data-stu-id="0f046-838">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="0f046-839">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="0f046-839">**DefaultValue**</span></span>                                                       | <span data-ttu-id="0f046-840">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-840">No</span></span>          | <span data-ttu-id="0f046-841">Der Standardwert der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="0f046-841">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="0f046-842">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="0f046-842">**MaxLength**</span></span>                                                          | <span data-ttu-id="0f046-843">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-843">No</span></span>          | <span data-ttu-id="0f046-844">Maximale Länge des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="0f046-844">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="0f046-845">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="0f046-845">**FixedLength**</span></span>                                                        | <span data-ttu-id="0f046-846">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-846">No</span></span>          | <span data-ttu-id="0f046-847">**True** oder **false** , abhängig davon, ob der Eigenschafts Wert als Zeichenfolge mit fester Länge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-847">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="0f046-848">**Genauigkeit**</span><span class="sxs-lookup"><span data-stu-id="0f046-848">**Precision**</span></span>                                                          | <span data-ttu-id="0f046-849">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-849">No</span></span>          | <span data-ttu-id="0f046-850">Die Genauigkeit des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="0f046-850">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="0f046-851">**Skalieren**</span><span class="sxs-lookup"><span data-stu-id="0f046-851">**Scale**</span></span>                                                              | <span data-ttu-id="0f046-852">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-852">No</span></span>          | <span data-ttu-id="0f046-853">Die Skalierung des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="0f046-853">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="0f046-854">**SRID**</span><span class="sxs-lookup"><span data-stu-id="0f046-854">**SRID**</span></span>                                                               | <span data-ttu-id="0f046-855">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-855">No</span></span>          | <span data-ttu-id="0f046-856">Verweis Bezeichner für räumliche Systeme.</span><span class="sxs-lookup"><span data-stu-id="0f046-856">Spatial System Reference Identifier.</span></span> <span data-ttu-id="0f046-857">Nur für Eigenschaften räumlicher Typen gültig.</span><span class="sxs-lookup"><span data-stu-id="0f046-857">Valid only for properties of spatial types.</span></span> <span data-ttu-id="0f046-858">Weitere Informationen finden Sie unter [SRID](https://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="0f046-858">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="0f046-859">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="0f046-859">**Unicode**</span></span>                                                            | <span data-ttu-id="0f046-860">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-860">No</span></span>          | <span data-ttu-id="0f046-861">**True** oder **false** , abhängig davon, ob der Eigenschafts Wert als Unicode-Zeichenfolge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-861">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="0f046-862">**Sortierung**</span><span class="sxs-lookup"><span data-stu-id="0f046-862">**Collation**</span></span>                                                          | <span data-ttu-id="0f046-863">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-863">No</span></span>          | <span data-ttu-id="0f046-864">Eine Zeichenfolge, die angibt, welche Sortierreihenfolge in der Datenquelle verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-864">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="0f046-865">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="0f046-865">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="0f046-866">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-866">No</span></span>          | <span data-ttu-id="0f046-867">**None** (Standardwert) oder **Fixed**.</span><span class="sxs-lookup"><span data-stu-id="0f046-867">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="0f046-868">Wenn der Wert auf **Fixed**festgelegt ist, wird der Wert der Eigenschaft zu Überprüfungen auf vollständige Parallelität verwendet.</span><span class="sxs-lookup"><span data-stu-id="0f046-868">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="0f046-869">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **Property** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-869">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="0f046-870">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-870">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-871">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-871">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="0f046-872">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-872">Example</span></span>

<span data-ttu-id="0f046-873">Das folgende Beispiel zeigt ein **EntityType** -Element mit drei **Eigenschafts** Elementen:</span><span class="sxs-lookup"><span data-stu-id="0f046-873">The following example shows an **EntityType** element with three **Property** elements:</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

<span data-ttu-id="0f046-874">Das folgende Beispiel zeigt ein **complexType** -Element mit fünf **Eigenschafts** Elementen:</span><span class="sxs-lookup"><span data-stu-id="0f046-874">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="0f046-875">RowType-Element-Anwendung</span><span class="sxs-lookup"><span data-stu-id="0f046-875">RowType Element Application</span></span>

<span data-ttu-id="0f046-876">**Eigenschafts** Elemente (als untergeordnete Elemente eines **RowType** -Elements) definieren die Form und die Eigenschaften von Daten, die an eine Modell definierte Funktion übermittelt oder von dieser zurückgegeben werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-876">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="0f046-877">Das **Property** -Element kann genau eines der folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="0f046-877">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="0f046-878">CollectionType</span><span class="sxs-lookup"><span data-stu-id="0f046-878">CollectionType</span></span>
-   <span data-ttu-id="0f046-879">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="0f046-879">ReferenceType</span></span>
-   <span data-ttu-id="0f046-880">RowType</span><span class="sxs-lookup"><span data-stu-id="0f046-880">RowType</span></span>

<span data-ttu-id="0f046-881">Das **Property** -Element kann eine beliebige Anzahl untergeordneter Anmerkung-Elemente aufweisen.</span><span class="sxs-lookup"><span data-stu-id="0f046-881">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="0f046-882">Annotation-Elemente sind nur in CSDL v2 und höher zulässig.</span><span class="sxs-lookup"><span data-stu-id="0f046-882">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="0f046-883">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-883">Applicable Attributes</span></span>

<span data-ttu-id="0f046-884">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **Property** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-884">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="0f046-885">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-885">Attribute Name</span></span>                                                     | <span data-ttu-id="0f046-886">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-886">Is Required</span></span> | <span data-ttu-id="0f046-887">value</span><span class="sxs-lookup"><span data-stu-id="0f046-887">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f046-888">**Name**</span><span class="sxs-lookup"><span data-stu-id="0f046-888">**Name**</span></span>                                                           | <span data-ttu-id="0f046-889">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-889">Yes</span></span>         | <span data-ttu-id="0f046-890">Der Name der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="0f046-890">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="0f046-891">**Typ**</span><span class="sxs-lookup"><span data-stu-id="0f046-891">**Type**</span></span>                                                           | <span data-ttu-id="0f046-892">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-892">Yes</span></span>         | <span data-ttu-id="0f046-893">Der Typ des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="0f046-893">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="0f046-894">**NULL zulassen**</span><span class="sxs-lookup"><span data-stu-id="0f046-894">**Nullable**</span></span>                                                       | <span data-ttu-id="0f046-895">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-895">No</span></span>          | <span data-ttu-id="0f046-896">**True** (Standardwert) oder **False** abhängig davon, ob die Eigenschaft einen NULL-Wert haben kann.</span><span class="sxs-lookup"><span data-stu-id="0f046-896">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="0f046-897">> In CSDL v1 muss eine Eigenschaft eines komplexen Typs über `Nullable="False"`verfügen.</span><span class="sxs-lookup"><span data-stu-id="0f046-897">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="0f046-898">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="0f046-898">**DefaultValue**</span></span>                                                   | <span data-ttu-id="0f046-899">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-899">No</span></span>          | <span data-ttu-id="0f046-900">Der Standardwert der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="0f046-900">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="0f046-901">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="0f046-901">**MaxLength**</span></span>                                                      | <span data-ttu-id="0f046-902">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-902">No</span></span>          | <span data-ttu-id="0f046-903">Maximale Länge des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="0f046-903">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="0f046-904">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="0f046-904">**FixedLength**</span></span>                                                    | <span data-ttu-id="0f046-905">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-905">No</span></span>          | <span data-ttu-id="0f046-906">**True** oder **false** , abhängig davon, ob der Eigenschafts Wert als Zeichenfolge mit fester Länge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-906">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="0f046-907">**Genauigkeit**</span><span class="sxs-lookup"><span data-stu-id="0f046-907">**Precision**</span></span>                                                      | <span data-ttu-id="0f046-908">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-908">No</span></span>          | <span data-ttu-id="0f046-909">Die Genauigkeit des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="0f046-909">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="0f046-910">**Skalieren**</span><span class="sxs-lookup"><span data-stu-id="0f046-910">**Scale**</span></span>                                                          | <span data-ttu-id="0f046-911">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-911">No</span></span>          | <span data-ttu-id="0f046-912">Die Skalierung des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="0f046-912">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="0f046-913">**SRID**</span><span class="sxs-lookup"><span data-stu-id="0f046-913">**SRID**</span></span>                                                           | <span data-ttu-id="0f046-914">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-914">No</span></span>          | <span data-ttu-id="0f046-915">Verweis Bezeichner für räumliche Systeme.</span><span class="sxs-lookup"><span data-stu-id="0f046-915">Spatial System Reference Identifier.</span></span> <span data-ttu-id="0f046-916">Nur für Eigenschaften räumlicher Typen gültig.</span><span class="sxs-lookup"><span data-stu-id="0f046-916">Valid only for properties of spatial types.</span></span> <span data-ttu-id="0f046-917">Weitere Informationen finden Sie unter [SRID](https://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="0f046-917">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="0f046-918">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="0f046-918">**Unicode**</span></span>                                                        | <span data-ttu-id="0f046-919">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-919">No</span></span>          | <span data-ttu-id="0f046-920">**True** oder **false** , abhängig davon, ob der Eigenschafts Wert als Unicode-Zeichenfolge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-920">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="0f046-921">**Sortierung**</span><span class="sxs-lookup"><span data-stu-id="0f046-921">**Collation**</span></span>                                                      | <span data-ttu-id="0f046-922">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-922">No</span></span>          | <span data-ttu-id="0f046-923">Eine Zeichenfolge, die angibt, welche Sortierreihenfolge in der Datenquelle verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-923">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="0f046-924">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **Property** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-924">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="0f046-925">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-925">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-926">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-926">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="0f046-927">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-927">Example</span></span>

<span data-ttu-id="0f046-928">Das folgende Beispiel zeigt **Eigenschafts** Elemente, die verwendet werden, um die Form des Rückgabe Typs einer Modell definierten Funktion zu definieren.</span><span class="sxs-lookup"><span data-stu-id="0f046-928">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="0f046-929">PropertyRef-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-929">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="0f046-930">Das **PropertyRef** -Element in konzeptioneller Schema Definitions Sprache (CSDL) verweist auf eine Eigenschaft eines Entitäts Typs, um anzugeben, dass die-Eigenschaft eine der folgenden Rollen ausführt:</span><span class="sxs-lookup"><span data-stu-id="0f046-930">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="0f046-931">Ein Teil des Entitätsschlüssels (eine Eigenschaft oder ein Satz von Eigenschaften eines Entitätstyps, der die Identität bestimmt).</span><span class="sxs-lookup"><span data-stu-id="0f046-931">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="0f046-932">Ein oder mehrere **PropertyRef** -Elemente können verwendet werden, um einen Entitäts Schlüssel zu definieren.</span><span class="sxs-lookup"><span data-stu-id="0f046-932">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="0f046-933">Das abhängige Ende oder Prinzipalende einer referenziellen Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="0f046-933">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="0f046-934">Das **PropertyRef** -Element kann nur Anmerkung-Elemente (0 (null) oder mehr) als untergeordnete Elemente aufweisen.</span><span class="sxs-lookup"><span data-stu-id="0f046-934">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="0f046-935">Annotation-Elemente sind nur in CSDL v2 und höher zulässig.</span><span class="sxs-lookup"><span data-stu-id="0f046-935">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-936">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-936">Applicable Attributes</span></span>

<span data-ttu-id="0f046-937">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **PropertyRef** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-937">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="0f046-938">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-938">Attribute Name</span></span> | <span data-ttu-id="0f046-939">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-939">Is Required</span></span> | <span data-ttu-id="0f046-940">value</span><span class="sxs-lookup"><span data-stu-id="0f046-940">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="0f046-941">**Name**</span><span class="sxs-lookup"><span data-stu-id="0f046-941">**Name**</span></span>       | <span data-ttu-id="0f046-942">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-942">Yes</span></span>         | <span data-ttu-id="0f046-943">Der Name der referenzierten Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="0f046-943">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f046-944">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **PropertyRef** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-944">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="0f046-945">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-945">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-946">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-946">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f046-947">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-947">Example</span></span>

<span data-ttu-id="0f046-948">Im folgenden Beispiel wird ein Entitätstyp (**Book**) definiert.</span><span class="sxs-lookup"><span data-stu-id="0f046-948">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="0f046-949">Der Entitäts Schlüssel wird definiert, indem auf die **ISBN** -Eigenschaft des Entitäts Typs verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-949">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

<span data-ttu-id="0f046-950">Im nächsten Beispiel werden zwei **PropertyRef** -Elemente verwendet, um anzugeben, dass zwei Eigenschaften (**ID** und **PublisherID**) das Prinzipal Ende und das abhängige Ende einer referenziellen Einschränkung sind.</span><span class="sxs-lookup"><span data-stu-id="0f046-950">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="0f046-951">ReferenceType-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-951">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="0f046-952">Das **ReferenceType** -Element in konzeptioneller Schema Definitions Sprache (CSDL) gibt einen Verweis auf einen Entitätstyp an.</span><span class="sxs-lookup"><span data-stu-id="0f046-952">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="0f046-953">Das **ReferenceType** -Element kann ein untergeordnetes Element der folgenden Elemente sein:</span><span class="sxs-lookup"><span data-stu-id="0f046-953">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="0f046-954">ReturnType (Funktion)</span><span class="sxs-lookup"><span data-stu-id="0f046-954">ReturnType (Function)</span></span>
-   <span data-ttu-id="0f046-955">Parameter</span><span class="sxs-lookup"><span data-stu-id="0f046-955">Parameter</span></span>
-   <span data-ttu-id="0f046-956">CollectionType</span><span class="sxs-lookup"><span data-stu-id="0f046-956">CollectionType</span></span>

<span data-ttu-id="0f046-957">Das **ReferenceType** -Element wird verwendet, wenn ein Parameter oder ein Rückgabetyp für eine Funktion definiert wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-957">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="0f046-958">Ein **ReferenceType** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="0f046-958">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f046-959">Dokumentation (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-959">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f046-960">Annotation-Elemente (0 (null) oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-960">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-961">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-961">Applicable Attributes</span></span>

<span data-ttu-id="0f046-962">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **ReferenceType** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-962">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="0f046-963">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-963">Attribute Name</span></span> | <span data-ttu-id="0f046-964">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-964">Is Required</span></span> | <span data-ttu-id="0f046-965">value</span><span class="sxs-lookup"><span data-stu-id="0f046-965">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="0f046-966">**Typ**</span><span class="sxs-lookup"><span data-stu-id="0f046-966">**Type**</span></span>       | <span data-ttu-id="0f046-967">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-967">Yes</span></span>         | <span data-ttu-id="0f046-968">Der Name des Entitätstyps, auf den verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-968">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f046-969">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **ReferenceType** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-969">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="0f046-970">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-970">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-971">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-971">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f046-972">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-972">Example</span></span>

<span data-ttu-id="0f046-973">Das folgende Beispiel zeigt das **ReferenceType** -Element, das als untergeordnetes Element eines **Parameter** -Elements in einer Modell definierten Funktion verwendet wird, die einen Verweis auf den Entitätstyp **Person** akzeptiert:</span><span class="sxs-lookup"><span data-stu-id="0f046-973">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
   <Parameter Name="instructor">
     <ReferenceType Type="SchoolModel.Person" />
   </Parameter>
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

<span data-ttu-id="0f046-974">Das folgende Beispiel zeigt das **ReferenceType** -Element, das als untergeordnetes Element eines **returnType** (Function)-Elements in einer Modell definierten Funktion verwendet wird, die einen Verweis auf einen **Person** -Entitätstyp zurückgibt:</span><span class="sxs-lookup"><span data-stu-id="0f046-974">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

``` xml
 <Function Name="GetPersonReference">
     <Parameter Name="p" Type="SchoolModel.Person" />
     <ReturnType>
         <ReferenceType Type="SchoolModel.Person" />
     </ReturnType>
     <DefiningExpression>
           REF(p)
     </DefiningExpression>
 </Function>
```
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="0f046-975">ReferentialConstraint-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-975">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="0f046-976">Ein **referentialeinschränkungs** -Element in konzeptioneller Schema Definitions Sprache (CSDL) definiert Funktionen, die einer Einschränkung der referenziellen Integrität in einer relationalen Datenbank ähneln.</span><span class="sxs-lookup"><span data-stu-id="0f046-976">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="0f046-977">So wie eine Spalte (oder Spalten) einer Datenbanktabelle auf den Primärschlüssel einer anderen Tabelle verweisen kann, kann eine Eigenschaft (oder Eigenschaften) eines Entitätstyps auf den Entitätsschlüssel eines anderen Entitätstyps verweisen.</span><span class="sxs-lookup"><span data-stu-id="0f046-977">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="0f046-978">Der Entitätstyp, auf den verwiesen wird, wird als *Prinzipal Ende* der Einschränkung bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="0f046-978">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="0f046-979">Der Entitätstyp, der auf das Prinzipal Ende verweist, wird als *abhängiges Ende* der Einschränkung bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="0f046-979">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="0f046-980">Wenn ein Fremdschlüssel, der auf einem Entitätstyp verfügbar gemacht wird, auf eine Eigenschaft eines anderen Entitäts Typs verweist, definiert das **referentialeinschränkung** -Element eine Zuordnung zwischen den beiden Entitäts Typen.</span><span class="sxs-lookup"><span data-stu-id="0f046-980">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="0f046-981">Da das **referentialeinschränkung** -Element Informationen über die Beziehung zweier Entitäts Typen bereitstellt, ist in der Mapping-Spezifikationssprache (MSL) kein entsprechendes AssociationSetMapping-Element erforderlich.</span><span class="sxs-lookup"><span data-stu-id="0f046-981">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="0f046-982">Eine Zuordnung zwischen zwei Entitäts Typen, für die keine Fremdschlüssel verfügbar gemacht werden, muss über ein entsprechendes **AssociationSetMapping** -Element verfügen, um der Datenquelle Zuordnungs Informationen zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="0f046-982">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="0f046-983">Wenn ein Fremdschlüssel nicht für einen Entitätstyp verfügbar gemacht wird, kann das **referentialeinschränkungs** -Element nur eine Primärschlüssel-zu-Primärschlüssel-Einschränkung zwischen dem Entitätstyp und einem anderen Entitätstyp definieren.</span><span class="sxs-lookup"><span data-stu-id="0f046-983">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="0f046-984">Ein **referentialeinschränkung** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="0f046-984">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f046-985">Dokumentation (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-985">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f046-986">Prinzipal (genau ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-986">Principal (exactly one element)</span></span>
-   <span data-ttu-id="0f046-987">Abhängig (genau ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-987">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="0f046-988">Annotation-Elemente (0 (null) oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-988">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-989">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-989">Applicable Attributes</span></span>

<span data-ttu-id="0f046-990">Das **referentialeinschränkungs** -Element kann über eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) verfügen.</span><span class="sxs-lookup"><span data-stu-id="0f046-990">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="0f046-991">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-991">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-992">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-992">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="0f046-993">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-993">Example</span></span>

<span data-ttu-id="0f046-994">Das folgende Beispiel zeigt ein **referenentialeinschränkungs** -Element, das als Teil der Definition der **PublishedBy** -Zuordnung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-994">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="0f046-995">ReturnType (Function)-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-995">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="0f046-996">Das **returnType** (Function)-Element in konzeptioneller Schema Definitions Sprache (CSDL) gibt den Rückgabetyp für eine Funktion an, die in einem Function-Element definiert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-996">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="0f046-997">Ein Funktions Rückgabetyp kann auch mit einem **returnType** -Attribut angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-997">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="0f046-998">Rückgabe Typen können alle **edmsimpletype**-Typen, Entitäts Typen, komplexe Typen, Zeilen Typen, Ref-Typen oder eine Auflistung von einem dieser Typen sein.</span><span class="sxs-lookup"><span data-stu-id="0f046-998">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="0f046-999">Der Rückgabetyp einer Funktion kann entweder mit dem **Type** -Attribut des **returnType** (Function)-Elements oder mit einem der folgenden untergeordneten Elemente angegeben werden:</span><span class="sxs-lookup"><span data-stu-id="0f046-999">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="0f046-1000">CollectionType</span><span class="sxs-lookup"><span data-stu-id="0f046-1000">CollectionType</span></span>
-   <span data-ttu-id="0f046-1001">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="0f046-1001">ReferenceType</span></span>
-   <span data-ttu-id="0f046-1002">RowType</span><span class="sxs-lookup"><span data-stu-id="0f046-1002">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="0f046-1003">Ein Modell wird nicht überprüft, wenn Sie einen Funktions Rückgabetyp mit dem **Type** -Attribut des **returnType** (Function)-Elements und einem der untergeordneten Elemente angeben.</span><span class="sxs-lookup"><span data-stu-id="0f046-1003">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-1004">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-1004">Applicable Attributes</span></span>

<span data-ttu-id="0f046-1005">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **returnType** (Function)-Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-1005">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="0f046-1006">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-1006">Attribute Name</span></span> | <span data-ttu-id="0f046-1007">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-1007">Is Required</span></span> | <span data-ttu-id="0f046-1008">value</span><span class="sxs-lookup"><span data-stu-id="0f046-1008">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="0f046-1009">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="0f046-1009">**ReturnType**</span></span> | <span data-ttu-id="0f046-1010">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-1010">No</span></span>          | <span data-ttu-id="0f046-1011">Der von der Funktion zurückgegebene Typ.</span><span class="sxs-lookup"><span data-stu-id="0f046-1011">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f046-1012">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **returnType** (Function)-Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-1012">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="0f046-1013">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-1013">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-1014">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-1014">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f046-1015">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-1015">Example</span></span>

<span data-ttu-id="0f046-1016">Im folgenden Beispiel wird ein **Function** -Element verwendet, um eine Funktion zu definieren, die die Anzahl der Jahre zurückgibt, in denen ein Buch gedruckt wurde.</span><span class="sxs-lookup"><span data-stu-id="0f046-1016">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="0f046-1017">Beachten Sie, dass der Rückgabetyp durch das **Type** -Attribut eines **returnType** (Function)-Elements angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-1017">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="0f046-1018">ReturnType (FunctionImport)-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-1018">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="0f046-1019">Das **returnType** (FunctionImport)-Element in konzeptioneller Schema Definitions Sprache (CSDL) gibt den Rückgabetyp für eine Funktion an, die in einem FunctionImport-Element definiert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-1019">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="0f046-1020">Ein Funktions Rückgabetyp kann auch mit einem **returnType** -Attribut angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-1020">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="0f046-1021">Rückgabe Typen können eine beliebige Auflistung von Entitäts Typen, komplexen Typen oder **edmsimpletype**sein.</span><span class="sxs-lookup"><span data-stu-id="0f046-1021">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="0f046-1022">Der Rückgabetyp einer Funktion wird mit dem **Type** -Attribut des **returnType** (FunctionImport)-Elements angegeben.</span><span class="sxs-lookup"><span data-stu-id="0f046-1022">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-1023">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-1023">Applicable Attributes</span></span>

<span data-ttu-id="0f046-1024">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **returnType** (FunctionImport)-Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-1024">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="0f046-1025">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-1025">Attribute Name</span></span> | <span data-ttu-id="0f046-1026">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-1026">Is Required</span></span> | <span data-ttu-id="0f046-1027">value</span><span class="sxs-lookup"><span data-stu-id="0f046-1027">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f046-1028">**Typ**</span><span class="sxs-lookup"><span data-stu-id="0f046-1028">**Type**</span></span>       | <span data-ttu-id="0f046-1029">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-1029">No</span></span>          | <span data-ttu-id="0f046-1030">Der Typ, den die Funktion zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="0f046-1030">The type that the function returns.</span></span> <span data-ttu-id="0f046-1031">Der Wert muss eine Auflistung von complexType, EntityType oder edmsimpletype sein.</span><span class="sxs-lookup"><span data-stu-id="0f046-1031">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="0f046-1032">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="0f046-1032">**EntitySet**</span></span>  | <span data-ttu-id="0f046-1033">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-1033">No</span></span>          | <span data-ttu-id="0f046-1034">Wenn die Funktion eine Auflistung von Entitäts Typen zurückgibt, muss der Wert von **EntitySet** der Entitätenmenge angehören, zu der die Auflistung gehört.</span><span class="sxs-lookup"><span data-stu-id="0f046-1034">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="0f046-1035">Andernfalls darf das Attribut **EntitySet** nicht verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-1035">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f046-1036">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **returnType** (FunctionImport)-Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-1036">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="0f046-1037">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-1037">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-1038">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-1038">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f046-1039">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-1039">Example</span></span>

<span data-ttu-id="0f046-1040">Im folgenden Beispiel wird ein **FunctionImport** -verwendet, der Bücher und Verleger zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="0f046-1040">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="0f046-1041">Beachten Sie, dass die Funktion zwei Resultsets zurückgibt. Daher werden zwei **returnType** (FunctionImport)-Elemente angegeben.</span><span class="sxs-lookup"><span data-stu-id="0f046-1041">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="0f046-1042">RowType-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-1042">RowType Element (CSDL)</span></span>

<span data-ttu-id="0f046-1043">Ein **RowType** -Element in der konzeptionellen Schema Definitions Sprache (CSDL) definiert eine unbenannte Struktur als Parameter oder Rückgabetyp für eine Funktion, die im konzeptionellen Modell definiert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-1043">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="0f046-1044">Ein **RowType** -Element kann das untergeordnete Element der folgenden Elemente sein:</span><span class="sxs-lookup"><span data-stu-id="0f046-1044">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="0f046-1045">CollectionType</span><span class="sxs-lookup"><span data-stu-id="0f046-1045">CollectionType</span></span>
-   <span data-ttu-id="0f046-1046">Parameter</span><span class="sxs-lookup"><span data-stu-id="0f046-1046">Parameter</span></span>
-   <span data-ttu-id="0f046-1047">ReturnType (Funktion)</span><span class="sxs-lookup"><span data-stu-id="0f046-1047">ReturnType (Function)</span></span>

<span data-ttu-id="0f046-1048">Ein **RowType** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="0f046-1048">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f046-1049">Eigenschaft (ein oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-1049">Property (one or more)</span></span>
-   <span data-ttu-id="0f046-1050">Anmerkungselemente (kein (null) oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-1050">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-1051">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-1051">Applicable Attributes</span></span>

<span data-ttu-id="0f046-1052">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **RowType** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-1052">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="0f046-1053">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-1053">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-1054">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-1054">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="0f046-1055">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-1055">Example</span></span>

<span data-ttu-id="0f046-1056">Das folgende Beispiel zeigt eine Modell definierte Funktion, die ein **CollectionType** -Element verwendet, um anzugeben, dass die Funktion eine Auflistung von Zeilen zurückgibt (wie im **RowType** -Element angegeben).</span><span class="sxs-lookup"><span data-stu-id="0f046-1056">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```

## <a name="schema-element-csdl"></a><span data-ttu-id="0f046-1057">Schema-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-1057">Schema Element (CSDL)</span></span>

<span data-ttu-id="0f046-1058">Das **Schema** -Element ist das Stamm Element einer konzeptionellen Modell Definition.</span><span class="sxs-lookup"><span data-stu-id="0f046-1058">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="0f046-1059">Es enthält Definitionen für die Objekte, Funktionen und Container, die ein konzeptionelles Modell bilden.</span><span class="sxs-lookup"><span data-stu-id="0f046-1059">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="0f046-1060">Das **Schema** Element kann NULL oder mehr der folgenden untergeordneten Elemente enthalten:</span><span class="sxs-lookup"><span data-stu-id="0f046-1060">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="0f046-1061">Verwenden</span><span class="sxs-lookup"><span data-stu-id="0f046-1061">Using</span></span>
-   <span data-ttu-id="0f046-1062">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="0f046-1062">EntityContainer</span></span>
-   <span data-ttu-id="0f046-1063">EntityType</span><span class="sxs-lookup"><span data-stu-id="0f046-1063">EntityType</span></span>
-   <span data-ttu-id="0f046-1064">EnumType</span><span class="sxs-lookup"><span data-stu-id="0f046-1064">EnumType</span></span>
-   <span data-ttu-id="0f046-1065">Zuordnung</span><span class="sxs-lookup"><span data-stu-id="0f046-1065">Association</span></span>
-   <span data-ttu-id="0f046-1066">ComplexType</span><span class="sxs-lookup"><span data-stu-id="0f046-1066">ComplexType</span></span>
-   <span data-ttu-id="0f046-1067">Funktion</span><span class="sxs-lookup"><span data-stu-id="0f046-1067">Function</span></span>

<span data-ttu-id="0f046-1068">Ein **Schema** Element kann NULL oder ein Element mit Anmerkungen enthalten.</span><span class="sxs-lookup"><span data-stu-id="0f046-1068">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="0f046-1069">Die Element-und Anmerkung-Elemente von **Funktionen** sind nur in CSDL v2 und höher zulässig.</span><span class="sxs-lookup"><span data-stu-id="0f046-1069">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="0f046-1070">Das **Schema** -Element verwendet das **Namespace** -Attribut, um den Namespace für den Entitätstyp, den komplexen Typ und die Zuordnungs Objekte in einem konzeptionellen Modell zu definieren.</span><span class="sxs-lookup"><span data-stu-id="0f046-1070">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="0f046-1071">Innerhalb eines Namespace müssen alle Objekte eine eindeutige Bezeichnung aufweisen.</span><span class="sxs-lookup"><span data-stu-id="0f046-1071">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="0f046-1072">Namespaces können mehrere **Schema** Elemente und mehrere CSDL-Dateien umfassen.</span><span class="sxs-lookup"><span data-stu-id="0f046-1072">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="0f046-1073">Ein Namespace des konzeptionellen Modells unterscheidet sich vom XML-Namespace des **Schema** -Elements.</span><span class="sxs-lookup"><span data-stu-id="0f046-1073">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="0f046-1074">Ein Namespace des konzeptionellen Modells (wie durch das **Namespace** -Attribut definiert) ist ein logischer Container für Entitäts Typen, komplexe Typen und Zuordnungs Typen.</span><span class="sxs-lookup"><span data-stu-id="0f046-1074">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="0f046-1075">Der XML-Namespace (angegeben durch das **xmlns** -Attribut) eines **Schema** -Elements ist der Standard Namespace für untergeordnete Elemente und Attribute des **Schema** -Elements.</span><span class="sxs-lookup"><span data-stu-id="0f046-1075">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="0f046-1076">XML-Namespaces der Form https://schemas.microsoft.com/ado/YYYY/MM/edm (wobei yyyy und mm jeweils ein Jahr und einen Monat darstellen) sind für CSDL reserviert.</span><span class="sxs-lookup"><span data-stu-id="0f046-1076">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="0f046-1077">Benutzerdefinierte Elemente und Attribute können nicht in Namespaces mit diesem Format vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="0f046-1077">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-1078">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-1078">Applicable Attributes</span></span>

<span data-ttu-id="0f046-1079">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **Schema** Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-1079">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="0f046-1080">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-1080">Attribute Name</span></span> | <span data-ttu-id="0f046-1081">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-1081">Is Required</span></span> | <span data-ttu-id="0f046-1082">value</span><span class="sxs-lookup"><span data-stu-id="0f046-1082">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f046-1083">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="0f046-1083">**Namespace**</span></span>  | <span data-ttu-id="0f046-1084">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-1084">Yes</span></span>         | <span data-ttu-id="0f046-1085">Der Namespace für das konzeptionelle Modell.</span><span class="sxs-lookup"><span data-stu-id="0f046-1085">The namespace of the conceptual model.</span></span> <span data-ttu-id="0f046-1086">Der Wert des **Namespace** -Attributs wird verwendet, um den voll qualifizierten Namen eines Typs zu bilden.</span><span class="sxs-lookup"><span data-stu-id="0f046-1086">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="0f046-1087">Wenn sich z. b. ein **EntityType** mit dem Namen *Customer* im Simple. example. Model-Namespace befindet, ist der voll qualifizierte Name des **EntityType** "simpleexamplemodel. Customer".</span><span class="sxs-lookup"><span data-stu-id="0f046-1087">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="0f046-1088">Die folgenden Zeichen folgen können nicht als Wert für das **Namespace** -Attribut verwendet werden: **System**, **transient**oder **EDM**.</span><span class="sxs-lookup"><span data-stu-id="0f046-1088">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="0f046-1089">Der Wert für das **Namespace** -Attribut darf nicht mit dem Wert für das **Namespace** -Attribut im SSDL-Schema Element identisch sein.</span><span class="sxs-lookup"><span data-stu-id="0f046-1089">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="0f046-1090">**Alias**</span><span class="sxs-lookup"><span data-stu-id="0f046-1090">**Alias**</span></span>      | <span data-ttu-id="0f046-1091">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-1091">No</span></span>          | <span data-ttu-id="0f046-1092">Ein anstelle der Namespacebezeichnung verwendeter Bezeichner.</span><span class="sxs-lookup"><span data-stu-id="0f046-1092">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="0f046-1093">Wenn z. b. ein **EntityType** mit dem Namen *Customer* im Simple. example. Model-Namespace und der Wert des **Alias** -Attributs *Model*ist, können Sie Model. Customer als voll qualifizierten Namen von **EntityType verwenden.**</span><span class="sxs-lookup"><span data-stu-id="0f046-1093">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="0f046-1094">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **Schema** Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-1094">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="0f046-1095">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-1095">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-1096">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-1096">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f046-1097">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-1097">Example</span></span>

<span data-ttu-id="0f046-1098">Das folgende Beispiel zeigt ein **Schema** -Element, das ein **EntityContainer** -Element, zwei **EntityType** -Elemente und ein **Association** -Element enthält.</span><span class="sxs-lookup"><span data-stu-id="0f046-1098">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
       Namespace="ExampleModel" Alias="Self">
         <EntityContainer Name="ExampleModelContainer">
           <EntitySet Name="Customers"
                      EntityType="ExampleModel.Customer" />
           <EntitySet Name="Orders" EntityType="ExampleModel.Order" />
           <AssociationSet
                       Name="CustomerOrder"
                       Association="ExampleModel.CustomerOrders">
             <End Role="Customer" EntitySet="Customers" />
             <End Role="Order" EntitySet="Orders" />
           </AssociationSet>
         </EntityContainer>
         <EntityType Name="Customer">
           <Key>
             <PropertyRef Name="CustomerId" />
           </Key>
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
           <Property Type="String" Name="Name" Nullable="false" />
           <NavigationProperty
                    Name="Orders"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Customer" ToRole="Order" />
         </EntityType>
         <EntityType Name="Order">
           <Key>
             <PropertyRef Name="OrderId" />
           </Key>
           <Property Type="Int32" Name="OrderId" Nullable="false" />
           <Property Type="Int32" Name="ProductId" Nullable="false" />
           <Property Type="Int32" Name="Quantity" Nullable="false" />
           <NavigationProperty
                    Name="Customer"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Order" ToRole="Customer" />
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
         </EntityType>
         <Association Name="CustomerOrders">
           <End Type="ExampleModel.Customer"
                Role="Customer" Multiplicity="1" />
           <End Type="ExampleModel.Order"
                Role="Order" Multiplicity="*" />
           <ReferentialConstraint>
             <Principal Role="Customer">
               <PropertyRef Name="CustomerId" />
             </Principal>
             <Dependent Role="Order">
               <PropertyRef Name="CustomerId" />
             </Dependent>
           </ReferentialConstraint>
         </Association>
       </Schema>
```
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="0f046-1099">TypeRef-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-1099">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="0f046-1100">Das **TypeRef** -Element in konzeptioneller Schema Definitions Sprache (CSDL) stellt einen Verweis auf einen vorhandenen benannten Typ bereit.</span><span class="sxs-lookup"><span data-stu-id="0f046-1100">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="0f046-1101">Das **TypeRef** -Element kann ein untergeordnetes Element des CollectionType-Elements sein, das verwendet wird, um anzugeben, dass eine Funktion eine Auflistung als Parameter oder Rückgabetyp aufweist.</span><span class="sxs-lookup"><span data-stu-id="0f046-1101">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="0f046-1102">Ein **TypeRef** -Element kann über die folgenden untergeordneten Elemente verfügen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="0f046-1102">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="0f046-1103">Dokumentation (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="0f046-1103">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="0f046-1104">Annotation-Elemente (0 (null) oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="0f046-1104">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-1105">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-1105">Applicable Attributes</span></span>

<span data-ttu-id="0f046-1106">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **TypeRef** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-1106">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="0f046-1107">Beachten Sie, dass die Attribute " **DefaultValue**", " **MaxLength**", " **FixedLength**", " **Precision**", " **Scale**", " **Unicode**" und " **COLLATIONS** " nur auf **edmsimple**</span><span class="sxs-lookup"><span data-stu-id="0f046-1107">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="0f046-1108">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-1108">Attribute Name</span></span>                                                     | <span data-ttu-id="0f046-1109">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-1109">Is Required</span></span> | <span data-ttu-id="0f046-1110">value</span><span class="sxs-lookup"><span data-stu-id="0f046-1110">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f046-1111">**Typ**</span><span class="sxs-lookup"><span data-stu-id="0f046-1111">**Type**</span></span>                                                           | <span data-ttu-id="0f046-1112">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-1112">No</span></span>          | <span data-ttu-id="0f046-1113">Der Name der Typbibliothek, auf die verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-1113">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="0f046-1114">**NULL zulassen**</span><span class="sxs-lookup"><span data-stu-id="0f046-1114">**Nullable**</span></span>                                                       | <span data-ttu-id="0f046-1115">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-1115">No</span></span>          | <span data-ttu-id="0f046-1116">**True** (Standardwert) oder **False** abhängig davon, ob die Eigenschaft einen NULL-Wert haben kann.</span><span class="sxs-lookup"><span data-stu-id="0f046-1116">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="0f046-1117">> In CSDL v1 muss eine Eigenschaft eines komplexen Typs über `Nullable="False"`verfügen.</span><span class="sxs-lookup"><span data-stu-id="0f046-1117">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="0f046-1118">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="0f046-1118">**DefaultValue**</span></span>                                                   | <span data-ttu-id="0f046-1119">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-1119">No</span></span>          | <span data-ttu-id="0f046-1120">Der Standardwert der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="0f046-1120">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="0f046-1121">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="0f046-1121">**MaxLength**</span></span>                                                      | <span data-ttu-id="0f046-1122">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-1122">No</span></span>          | <span data-ttu-id="0f046-1123">Maximale Länge des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="0f046-1123">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="0f046-1124">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="0f046-1124">**FixedLength**</span></span>                                                    | <span data-ttu-id="0f046-1125">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-1125">No</span></span>          | <span data-ttu-id="0f046-1126">**True** oder **false** , abhängig davon, ob der Eigenschafts Wert als Zeichenfolge mit fester Länge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-1126">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="0f046-1127">**Genauigkeit**</span><span class="sxs-lookup"><span data-stu-id="0f046-1127">**Precision**</span></span>                                                      | <span data-ttu-id="0f046-1128">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-1128">No</span></span>          | <span data-ttu-id="0f046-1129">Die Genauigkeit des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="0f046-1129">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="0f046-1130">**Skalieren**</span><span class="sxs-lookup"><span data-stu-id="0f046-1130">**Scale**</span></span>                                                          | <span data-ttu-id="0f046-1131">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-1131">No</span></span>          | <span data-ttu-id="0f046-1132">Die Skalierung des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="0f046-1132">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="0f046-1133">**SRID**</span><span class="sxs-lookup"><span data-stu-id="0f046-1133">**SRID**</span></span>                                                           | <span data-ttu-id="0f046-1134">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-1134">No</span></span>          | <span data-ttu-id="0f046-1135">Verweis Bezeichner für räumliche Systeme.</span><span class="sxs-lookup"><span data-stu-id="0f046-1135">Spatial System Reference Identifier.</span></span> <span data-ttu-id="0f046-1136">Nur für Eigenschaften räumlicher Typen gültig.</span><span class="sxs-lookup"><span data-stu-id="0f046-1136">Valid only for properties of spatial types.</span></span> <span data-ttu-id="0f046-1137">Weitere Informationen finden Sie unter [SRID](https://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="0f046-1137">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="0f046-1138">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="0f046-1138">**Unicode**</span></span>                                                        | <span data-ttu-id="0f046-1139">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-1139">No</span></span>          | <span data-ttu-id="0f046-1140">**True** oder **false** , abhängig davon, ob der Eigenschafts Wert als Unicode-Zeichenfolge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-1140">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="0f046-1141">**Sortierung**</span><span class="sxs-lookup"><span data-stu-id="0f046-1141">**Collation**</span></span>                                                      | <span data-ttu-id="0f046-1142">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-1142">No</span></span>          | <span data-ttu-id="0f046-1143">Eine Zeichenfolge, die angibt, welche Sortierreihenfolge in der Datenquelle verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-1143">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="0f046-1144">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **CollectionType** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-1144">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="0f046-1145">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-1145">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-1146">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-1146">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f046-1147">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-1147">Example</span></span>

<span data-ttu-id="0f046-1148">Das folgende Beispiel zeigt eine Modell definierte Funktion, die das **TypeRef** -Element verwendet (als untergeordnetes Element eines **CollectionType** -Elements), um anzugeben, dass die Funktion eine Auflistung von **Abteilungs** Entitäts Typen akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="0f046-1148">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="0f046-1149">Using-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-1149">Using Element (CSDL)</span></span>

<span data-ttu-id="0f046-1150">Das **using** -Element in konzeptioneller Schema Definitions Sprache (CSDL) importiert den Inhalt eines konzeptionellen Modells, das in einem anderen Namespace vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-1150">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="0f046-1151">Wenn Sie den Wert des **Namespace** -Attributs festlegen, können Sie auf Entitäts Typen, komplexe Typen und Zuordnungs Typen verweisen, die in einem anderen konzeptionellen Modell definiert sind.</span><span class="sxs-lookup"><span data-stu-id="0f046-1151">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="0f046-1152">Mehr als ein **using** -Element kann ein untergeordnetes Element eines **Schema** -Elements sein.</span><span class="sxs-lookup"><span data-stu-id="0f046-1152">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="0f046-1153">Das **using** -Element in CSDL funktioniert nicht genau wie eine **using** -Anweisung in einer Programmiersprache.</span><span class="sxs-lookup"><span data-stu-id="0f046-1153">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="0f046-1154">Wenn Sie einen Namespace mit einer **using** -Anweisung in einer Programmiersprache importieren, wirkt sich dies nicht auf Objekte im ursprünglichen Namespace aus.</span><span class="sxs-lookup"><span data-stu-id="0f046-1154">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="0f046-1155">In CSDL kann ein importierter Namespace einen Entitätstyp enthalten, der von einem Entitätstyp im ursprünglichen Namespace abgeleitet ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-1155">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="0f046-1156">Dies kann sich auf im ursprünglichen Namespace deklarierte Entitätssätze auswirken.</span><span class="sxs-lookup"><span data-stu-id="0f046-1156">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="0f046-1157">Das **using** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="0f046-1157">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="0f046-1158">Dokumentation (kein (null) oder ein Element zulässig)</span><span class="sxs-lookup"><span data-stu-id="0f046-1158">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="0f046-1159">Annotation-Elemente (0 (null) oder mehr Elemente zulässig)</span><span class="sxs-lookup"><span data-stu-id="0f046-1159">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="0f046-1160">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="0f046-1160">Applicable Attributes</span></span>

<span data-ttu-id="0f046-1161">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **using** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-1161">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="0f046-1162">Attributname</span><span class="sxs-lookup"><span data-stu-id="0f046-1162">Attribute Name</span></span> | <span data-ttu-id="0f046-1163">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="0f046-1163">Is Required</span></span> | <span data-ttu-id="0f046-1164">value</span><span class="sxs-lookup"><span data-stu-id="0f046-1164">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="0f046-1165">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="0f046-1165">**Namespace**</span></span>  | <span data-ttu-id="0f046-1166">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-1166">Yes</span></span>         | <span data-ttu-id="0f046-1167">Der Name des importierten Namespaces.</span><span class="sxs-lookup"><span data-stu-id="0f046-1167">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="0f046-1168">**Alias**</span><span class="sxs-lookup"><span data-stu-id="0f046-1168">**Alias**</span></span>      | <span data-ttu-id="0f046-1169">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-1169">Yes</span></span>         | <span data-ttu-id="0f046-1170">Ein anstelle der Namespacebezeichnung verwendeter Bezeichner.</span><span class="sxs-lookup"><span data-stu-id="0f046-1170">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="0f046-1171">Obwohl dieses Attribut erforderlich ist, muss es nicht anstelle des Namespacenamens verwendet wird, um Objektnamen zu qualifizieren.</span><span class="sxs-lookup"><span data-stu-id="0f046-1171">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="0f046-1172">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **using** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-1172">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="0f046-1173">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-1173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="0f046-1174">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-1174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="0f046-1175">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-1175">Example</span></span>

<span data-ttu-id="0f046-1176">Das folgende Beispiel veranschaulicht das **using** -Element, das verwendet wird, um einen Namespace zu importieren, der an anderer Stelle definiert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-1176">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="0f046-1177">Beachten Sie, dass der Namespace für das angezeigte **Schema** Element `BooksModel`ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-1177">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="0f046-1178">Die `Address`-Eigenschaft für den `Publisher`**EntityType** ist ein komplexer Typ, der im `ExtendedBooksModel`-Namespace definiert ist (importiert mit dem **using** -Element).</span><span class="sxs-lookup"><span data-stu-id="0f046-1178">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
           Namespace="BooksModel" Alias="Self">

     <Using Namespace="BooksModel.Extended" Alias="BMExt" />

 <EntityContainer Name="BooksContainer" >
       <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
     </EntityContainer>

 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BMExt.Address" Name="Address" Nullable="false" />
     </EntityType>

 </Schema>
```
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="0f046-1179">Anmerkungsattribute (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-1179">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="0f046-1180">Anmerkungsattribute in konzeptioneller Schemadefinitionssprache (CSDL) sind benutzerdefinierte XML-Attribute im konzeptionellen Modell.</span><span class="sxs-lookup"><span data-stu-id="0f046-1180">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="0f046-1181">Neben dem Vorhandensein einer gültigen XML-Struktur muss für Anmerkungsattribute folgendes zutreffen:</span><span class="sxs-lookup"><span data-stu-id="0f046-1181">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="0f046-1182">Anmerkungsattribute dürfen sich in keinem XML-Namespace befinden, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-1182">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="0f046-1183">Für ein angegebenes CSDL-Element kann mehr als ein Anmerkungsattribut übernommen werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-1183">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="0f046-1184">Die vollqualifizierten Namen zweier Anmerkungsattribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-1184">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="0f046-1185">Anmerkungsattribute können verwendet werden, um zusätzliche Metadaten für die Elemente in einem konzeptionellen Modell bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="0f046-1185">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="0f046-1186">Auf Metadaten, die in Anmerkung-Elementen enthalten sind, kann zur Laufzeit mithilfe von Klassen im System. Data. Metadata. Edm-Namespace zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-1186">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="0f046-1187">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-1187">Example</span></span>

<span data-ttu-id="0f046-1188">Das folgende Beispiel zeigt ein **EntityType** -Element mit einem Anmerkung-Attribut (**CustomAttribute**).</span><span class="sxs-lookup"><span data-stu-id="0f046-1188">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="0f046-1189">Das Beispiel zeigt außerdem ein Anmerkungselement, das auf das Entitätstypelement angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-1189">The example also shows an annotation element applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

<span data-ttu-id="0f046-1190">Im folgenden Code werden die Metadaten im Anmerkungsattribut abgerufen und in die Konsole geschrieben:</span><span class="sxs-lookup"><span data-stu-id="0f046-1190">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

``` xml
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomAttribute"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomAttribute"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

<span data-ttu-id="0f046-1191">Im Code oben wird davon ausgegangen, dass sich die Datei `School.csdl` im Ausgabeverzeichnis des Projekts befindet und dass Sie dem Projekt die folgenden `Imports`- und `Using`-Anweisungen hinzugefügt haben:</span><span class="sxs-lookup"><span data-stu-id="0f046-1191">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="0f046-1192">Anmerkungselemente (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-1192">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="0f046-1193">Anmerkungselemente in konzeptioneller Schemadefinitionssprache (CSDL) sind benutzerdefinierte XML-Elemente im konzeptionellen Modell.</span><span class="sxs-lookup"><span data-stu-id="0f046-1193">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="0f046-1194">Neben dem Vorhandensein einer gültigen XML-Struktur muss für Anmerkungselemente Folgendes zutreffen:</span><span class="sxs-lookup"><span data-stu-id="0f046-1194">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="0f046-1195">Anmerkungselemente dürfen sich in keinem XML-Namespace befinden, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="0f046-1195">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="0f046-1196">Mehrere Anmerkungselemente können untergeordnete Elemente eines angegebenen CSDL-Elements sein.</span><span class="sxs-lookup"><span data-stu-id="0f046-1196">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="0f046-1197">Die vollqualifizierten Namen zweier Anmerkungselemente dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="0f046-1197">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="0f046-1198">Anmerkungselemente müssen nach allen anderen untergeordneten Elementen eines angegebenen CSDL-Elements angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-1198">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="0f046-1199">Anmerkungselemente können verwendet werden, um zusätzliche Metadaten für die Elemente in einem konzeptionellen Modell bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="0f046-1199">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="0f046-1200">Ab Version 4 von .NET Framework können mithilfe von Klassen im Namespace "System. Data. Metadata. Edm" zur Laufzeit auf Metadaten zugegriffen werden, die in Anmerkung-Elementen enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="0f046-1200">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="0f046-1201">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-1201">Example</span></span>

<span data-ttu-id="0f046-1202">Das folgende Beispiel zeigt ein **EntityType** -Element mit einem Anmerkung-Element (**customelement**).</span><span class="sxs-lookup"><span data-stu-id="0f046-1202">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="0f046-1203">Das Beispiel zeigt außerdem ein Anmerkungsattribut, das auf das Entitätstypelement angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-1203">The example also show an annotation attribute applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

<span data-ttu-id="0f046-1204">Im folgenden Code werden die Metadaten im Anmerkungselement abgerufen und in die Konsole geschrieben:</span><span class="sxs-lookup"><span data-stu-id="0f046-1204">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

``` csharp
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomElement"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomElement"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

<span data-ttu-id="0f046-1205">Im Code oben wird davon ausgegangen, dass sich die Datei School.csdl im Ausgabeverzeichnis des Projekts befindet, und dass Sie dem Projekt die folgenden `Imports`- und `Using`-Anweisungen hinzugefügt haben:</span><span class="sxs-lookup"><span data-stu-id="0f046-1205">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="0f046-1206">Konzeptionelle Modelltypen (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-1206">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="0f046-1207">Die konzeptionelle Schema Definitions Sprache (CSDL) unterstützt einen Satz abstrakter primitiver Datentypen namens **edmsimpletypes**, die Eigenschaften in einem konzeptionellen Modell definieren.</span><span class="sxs-lookup"><span data-stu-id="0f046-1207">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="0f046-1208">**Edmsimpletypes** sind Proxys für primitive Datentypen, die in der Speicher-oder Hostingumgebung unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="0f046-1208">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="0f046-1209">In der nachfolgenden Tabelle werden die von CSDL unterstützten primitiven Datentypen aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="0f046-1209">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="0f046-1210">In der Tabelle werden auch die Facetten aufgelistet, die auf jeden **edmsimpletype**angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="0f046-1210">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="0f046-1211">EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="0f046-1211">EDMSimpleType</span></span>                    | <span data-ttu-id="0f046-1212">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="0f046-1212">Description</span></span>                                                | <span data-ttu-id="0f046-1213">Anwendbare Facets</span><span class="sxs-lookup"><span data-stu-id="0f046-1213">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="0f046-1214">**EDM. Binary**</span><span class="sxs-lookup"><span data-stu-id="0f046-1214">**Edm.Binary**</span></span>                   | <span data-ttu-id="0f046-1215">Enthält Binärdaten.</span><span class="sxs-lookup"><span data-stu-id="0f046-1215">Contains binary data.</span></span>                                      | <span data-ttu-id="0f046-1216">MaxLength, FixedLength, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="0f046-1216">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="0f046-1217">**Edm.Boolean**</span><span class="sxs-lookup"><span data-stu-id="0f046-1217">**Edm.Boolean**</span></span>                  | <span data-ttu-id="0f046-1218">Enthält den Wert **true** oder **false**.</span><span class="sxs-lookup"><span data-stu-id="0f046-1218">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="0f046-1219">Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="0f046-1219">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="0f046-1220">**EDM. Byte**</span><span class="sxs-lookup"><span data-stu-id="0f046-1220">**Edm.Byte**</span></span>                     | <span data-ttu-id="0f046-1221">Enthält einen 8-Bit-Ganzzahlwert ohne Vorzeichen.</span><span class="sxs-lookup"><span data-stu-id="0f046-1221">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="0f046-1222">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="0f046-1222">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f046-1223">**EDM. DateTime**</span><span class="sxs-lookup"><span data-stu-id="0f046-1223">**Edm.DateTime**</span></span>                 | <span data-ttu-id="0f046-1224">Stellt ein Datum und eine Uhrzeit dar.</span><span class="sxs-lookup"><span data-stu-id="0f046-1224">Represents a date and time.</span></span>                                | <span data-ttu-id="0f046-1225">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="0f046-1225">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f046-1226">**Edm.DateTimeOffset**</span><span class="sxs-lookup"><span data-stu-id="0f046-1226">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="0f046-1227">Enthält ein Datum und eine Uhrzeit als Versatz von UTC in Minuten.</span><span class="sxs-lookup"><span data-stu-id="0f046-1227">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="0f046-1228">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="0f046-1228">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f046-1229">**EDM. Decimal**</span><span class="sxs-lookup"><span data-stu-id="0f046-1229">**Edm.Decimal**</span></span>                  | <span data-ttu-id="0f046-1230">Enthält einen numerischen Wert mit fester Genauigkeit und festen Dezimalstellen.</span><span class="sxs-lookup"><span data-stu-id="0f046-1230">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="0f046-1231">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="0f046-1231">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f046-1232">**Edm.Double**</span><span class="sxs-lookup"><span data-stu-id="0f046-1232">**Edm.Double**</span></span>                   | <span data-ttu-id="0f046-1233">Enthält eine Gleit Komma Zahl mit einer Genauigkeit von 15 Ziffern.</span><span class="sxs-lookup"><span data-stu-id="0f046-1233">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="0f046-1234">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="0f046-1234">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f046-1235">**EDM. float**</span><span class="sxs-lookup"><span data-stu-id="0f046-1235">**Edm.Float**</span></span>                    | <span data-ttu-id="0f046-1236">Enthält eine Gleitkommazahl mit einer Genauigkeit von 7 Stellen.</span><span class="sxs-lookup"><span data-stu-id="0f046-1236">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="0f046-1237">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="0f046-1237">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f046-1238">**EDM. GUID**</span><span class="sxs-lookup"><span data-stu-id="0f046-1238">**Edm.Guid**</span></span>                     | <span data-ttu-id="0f046-1239">Enthält einen eindeutigen 16-Byte-Bezeichner.</span><span class="sxs-lookup"><span data-stu-id="0f046-1239">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="0f046-1240">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="0f046-1240">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f046-1241">**EDM. Int16**</span><span class="sxs-lookup"><span data-stu-id="0f046-1241">**Edm.Int16**</span></span>                    | <span data-ttu-id="0f046-1242">Enthält einen 16-Bit-Ganzzahlwert mit Vorzeichen.</span><span class="sxs-lookup"><span data-stu-id="0f046-1242">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="0f046-1243">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="0f046-1243">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f046-1244">**Edm.Int32**</span><span class="sxs-lookup"><span data-stu-id="0f046-1244">**Edm.Int32**</span></span>                    | <span data-ttu-id="0f046-1245">Enthält einen 32-Bit-Ganzzahlwert mit Vorzeichen.</span><span class="sxs-lookup"><span data-stu-id="0f046-1245">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="0f046-1246">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="0f046-1246">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f046-1247">**Edm.Int64**</span><span class="sxs-lookup"><span data-stu-id="0f046-1247">**Edm.Int64**</span></span>                    | <span data-ttu-id="0f046-1248">Enthält einen 64-Bit-Ganzzahlwert mit Vorzeichen.</span><span class="sxs-lookup"><span data-stu-id="0f046-1248">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="0f046-1249">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="0f046-1249">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f046-1250">**EDM. SByte**</span><span class="sxs-lookup"><span data-stu-id="0f046-1250">**Edm.SByte**</span></span>                    | <span data-ttu-id="0f046-1251">Enthält einen 8-Bit-Ganzzahlwert mit Vorzeichen.</span><span class="sxs-lookup"><span data-stu-id="0f046-1251">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="0f046-1252">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="0f046-1252">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f046-1253">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="0f046-1253">**Edm.String**</span></span>                   | <span data-ttu-id="0f046-1254">Enthält Zeichendaten.</span><span class="sxs-lookup"><span data-stu-id="0f046-1254">Contains character data.</span></span>                                   | <span data-ttu-id="0f046-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="0f046-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="0f046-1256">**EDM. Zeit**</span><span class="sxs-lookup"><span data-stu-id="0f046-1256">**Edm.Time**</span></span>                     | <span data-ttu-id="0f046-1257">Enthält eine Uhrzeit.</span><span class="sxs-lookup"><span data-stu-id="0f046-1257">Contains a time of day.</span></span>                                    | <span data-ttu-id="0f046-1258">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="0f046-1258">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="0f046-1259">**EDM. geography**</span><span class="sxs-lookup"><span data-stu-id="0f046-1259">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="0f046-1260">Nullable, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="0f046-1260">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f046-1261">**Edm.GeographyPoint**</span><span class="sxs-lookup"><span data-stu-id="0f046-1261">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="0f046-1262">Nullable, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="0f046-1262">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f046-1263">**EDM. geographylinestring**</span><span class="sxs-lookup"><span data-stu-id="0f046-1263">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="0f046-1264">Nullable, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="0f046-1264">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f046-1265">**EDM. geographypolygon**</span><span class="sxs-lookup"><span data-stu-id="0f046-1265">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="0f046-1266">Nullable, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="0f046-1266">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f046-1267">**EDM. geographymultipoint**</span><span class="sxs-lookup"><span data-stu-id="0f046-1267">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="0f046-1268">Nullable, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="0f046-1268">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f046-1269">**EDM. geographymultilinestring**</span><span class="sxs-lookup"><span data-stu-id="0f046-1269">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="0f046-1270">Nullable, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="0f046-1270">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f046-1271">**EDM. geographymultipolygon**</span><span class="sxs-lookup"><span data-stu-id="0f046-1271">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="0f046-1272">Nullable, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="0f046-1272">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f046-1273">**EDM. geographycollection**</span><span class="sxs-lookup"><span data-stu-id="0f046-1273">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="0f046-1274">Nullable, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="0f046-1274">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f046-1275">**EDM. Geometry**</span><span class="sxs-lookup"><span data-stu-id="0f046-1275">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="0f046-1276">Nullable, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="0f046-1276">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f046-1277">**EDM. geometryPoint**</span><span class="sxs-lookup"><span data-stu-id="0f046-1277">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="0f046-1278">Nullable, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="0f046-1278">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f046-1279">**EDM. geometrylinestring**</span><span class="sxs-lookup"><span data-stu-id="0f046-1279">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="0f046-1280">Nullable, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="0f046-1280">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f046-1281">**EDM. geometrypolygon**</span><span class="sxs-lookup"><span data-stu-id="0f046-1281">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="0f046-1282">Nullable, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="0f046-1282">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f046-1283">**EDM. geometrymultipoint**</span><span class="sxs-lookup"><span data-stu-id="0f046-1283">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="0f046-1284">Nullable, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="0f046-1284">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f046-1285">**EDM. geometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="0f046-1285">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="0f046-1286">Nullable, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="0f046-1286">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f046-1287">**EDM. geometrymultipolygon**</span><span class="sxs-lookup"><span data-stu-id="0f046-1287">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="0f046-1288">Nullable, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="0f046-1288">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="0f046-1289">**EDM. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="0f046-1289">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="0f046-1290">Nullable, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="0f046-1290">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="0f046-1291">Facets (CSDL)</span><span class="sxs-lookup"><span data-stu-id="0f046-1291">Facets (CSDL)</span></span>

<span data-ttu-id="0f046-1292">Facets in konzeptioneller Schemadefinitionssprache (CSDL) stellen Einschränkungen für Eigenschaften von Entitätstypen und komplexen Typen dar.</span><span class="sxs-lookup"><span data-stu-id="0f046-1292">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="0f046-1293">Facets werden in den folgenden CSDL-Elementen als XML-Attribute angezeigt:</span><span class="sxs-lookup"><span data-stu-id="0f046-1293">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="0f046-1294">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="0f046-1294">Property</span></span>
-   <span data-ttu-id="0f046-1295">TypeRef</span><span class="sxs-lookup"><span data-stu-id="0f046-1295">TypeRef</span></span>
-   <span data-ttu-id="0f046-1296">Parameter</span><span class="sxs-lookup"><span data-stu-id="0f046-1296">Parameter</span></span>

<span data-ttu-id="0f046-1297">In der folgenden Tabelle werden die in CSDL unterstützten Facets beschrieben.</span><span class="sxs-lookup"><span data-stu-id="0f046-1297">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="0f046-1298">Alle Facets sind optional.</span><span class="sxs-lookup"><span data-stu-id="0f046-1298">All facets are optional.</span></span> <span data-ttu-id="0f046-1299">Einige der unten aufgeführten Aspekte werden vom Entity Framework beim Erstellen einer Datenbank aus einem konzeptionellen Modell verwendet.</span><span class="sxs-lookup"><span data-stu-id="0f046-1299">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="0f046-1300">Informationen zu Datentypen in einem konzeptionellen Modell finden Sie unter konzeptionelle Modelltypen (CSDL).</span><span class="sxs-lookup"><span data-stu-id="0f046-1300">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="0f046-1301">Facet</span><span class="sxs-lookup"><span data-stu-id="0f046-1301">Facet</span></span>               | <span data-ttu-id="0f046-1302">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="0f046-1302">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="0f046-1303">Anwendungsbereich</span><span class="sxs-lookup"><span data-stu-id="0f046-1303">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="0f046-1304">Wird für die Datenbankgenerierung verwendet</span><span class="sxs-lookup"><span data-stu-id="0f046-1304">Used for the database generation</span></span> | <span data-ttu-id="0f046-1305">Wird von der Laufzeit verwendet</span><span class="sxs-lookup"><span data-stu-id="0f046-1305">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="0f046-1306">**Sortierung**</span><span class="sxs-lookup"><span data-stu-id="0f046-1306">**Collation**</span></span>       | <span data-ttu-id="0f046-1307">Gibt die bei Vergleich- und Sortiervorgängen zu verwendende Sortierreihenfolge für die Werte der Eigenschaft an.</span><span class="sxs-lookup"><span data-stu-id="0f046-1307">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="0f046-1308">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="0f046-1308">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="0f046-1309">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-1309">Yes</span></span>                              | <span data-ttu-id="0f046-1310">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-1310">No</span></span>                  |
| <span data-ttu-id="0f046-1311">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="0f046-1311">**ConcurrencyMode**</span></span> | <span data-ttu-id="0f046-1312">Gibt an, dass der Eigenschaftswert für Prüfungen der vollständigen Parallelität verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="0f046-1312">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="0f046-1313">Alle **edmsimpletype** -Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="0f046-1313">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="0f046-1314">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-1314">No</span></span>                               | <span data-ttu-id="0f046-1315">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-1315">Yes</span></span>                 |
| <span data-ttu-id="0f046-1316">**Standard**</span><span class="sxs-lookup"><span data-stu-id="0f046-1316">**Default**</span></span>         | <span data-ttu-id="0f046-1317">Gibt den Standardwert der Eigenschaft an, wenn bei der Instanziierung kein Wert angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-1317">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="0f046-1318">Alle **edmsimpletype** -Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="0f046-1318">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="0f046-1319">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-1319">Yes</span></span>                              | <span data-ttu-id="0f046-1320">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-1320">Yes</span></span>                 |
| <span data-ttu-id="0f046-1321">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="0f046-1321">**FixedLength**</span></span>     | <span data-ttu-id="0f046-1322">Gibt an, ob sich die Länge des Eigenschaftswerts ändern kann.</span><span class="sxs-lookup"><span data-stu-id="0f046-1322">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="0f046-1323">**Edm. Binary**, **Edm. String**</span><span class="sxs-lookup"><span data-stu-id="0f046-1323">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="0f046-1324">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-1324">Yes</span></span>                              | <span data-ttu-id="0f046-1325">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-1325">No</span></span>                  |
| <span data-ttu-id="0f046-1326">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="0f046-1326">**MaxLength**</span></span>       | <span data-ttu-id="0f046-1327">Gibt die maximale Länge des Eigenschaftswerts an.</span><span class="sxs-lookup"><span data-stu-id="0f046-1327">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="0f046-1328">**Edm. Binary**, **Edm. String**</span><span class="sxs-lookup"><span data-stu-id="0f046-1328">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="0f046-1329">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-1329">Yes</span></span>                              | <span data-ttu-id="0f046-1330">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-1330">No</span></span>                  |
| <span data-ttu-id="0f046-1331">**NULL zulassen**</span><span class="sxs-lookup"><span data-stu-id="0f046-1331">**Nullable**</span></span>        | <span data-ttu-id="0f046-1332">Gibt an, ob die Eigenschaft einen **null** -Wert aufweisen kann.</span><span class="sxs-lookup"><span data-stu-id="0f046-1332">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="0f046-1333">Alle **edmsimpletype** -Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="0f046-1333">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="0f046-1334">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-1334">Yes</span></span>                              | <span data-ttu-id="0f046-1335">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-1335">Yes</span></span>                 |
| <span data-ttu-id="0f046-1336">**Genauigkeit**</span><span class="sxs-lookup"><span data-stu-id="0f046-1336">**Precision**</span></span>       | <span data-ttu-id="0f046-1337">Gibt bei Eigenschaften vom Typ **Decimal**die Anzahl der Ziffern an, die ein Eigenschafts Wert aufweisen kann.</span><span class="sxs-lookup"><span data-stu-id="0f046-1337">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="0f046-1338">Bei Eigenschaften vom Typ **time**, **DateTime**und **DateTimeOffset**wird die Anzahl der Ziffern für den Bruchteil der Sekunden des Eigenschafts Werts angegeben.</span><span class="sxs-lookup"><span data-stu-id="0f046-1338">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="0f046-1339">**Edm. DateTime**, **Edm. DateTimeOffset**, **Edm. Decimal**, **Edm. Time**</span><span class="sxs-lookup"><span data-stu-id="0f046-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="0f046-1340">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-1340">Yes</span></span>                              | <span data-ttu-id="0f046-1341">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-1341">No</span></span>                  |
| <span data-ttu-id="0f046-1342">**Skalieren**</span><span class="sxs-lookup"><span data-stu-id="0f046-1342">**Scale**</span></span>           | <span data-ttu-id="0f046-1343">Gibt die Anzahl der Dezimalstellen für den Eigenschaftswert an.</span><span class="sxs-lookup"><span data-stu-id="0f046-1343">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="0f046-1344">**EDM. Decimal**</span><span class="sxs-lookup"><span data-stu-id="0f046-1344">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="0f046-1345">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-1345">Yes</span></span>                              | <span data-ttu-id="0f046-1346">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-1346">No</span></span>                  |
| <span data-ttu-id="0f046-1347">**SRID**</span><span class="sxs-lookup"><span data-stu-id="0f046-1347">**SRID**</span></span>            | <span data-ttu-id="0f046-1348">Gibt die System-ID des räumlichen System Verweises an.</span><span class="sxs-lookup"><span data-stu-id="0f046-1348">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="0f046-1349">Weitere Informationen finden Sie unter [SRID](https://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="0f046-1349">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="0f046-1350">**EDM. Geography, EDM. geographypoint, EDM. geographylinestring, EDM. geographypolygon, EDM. geographymultipoint, EDM. geographymultilinestring, EDM. geographymultipolygon, EDM. geographycollection, EDM. Geometry, EDM. geometryPoint, EDM. geometrylinestring, EDM. geometrypolygon, EDM. geometrymultipoint, EDM. geometryMultiLineString, EDM. geometrymultipolygon, EDM. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="0f046-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="0f046-1351">Nein</span><span class="sxs-lookup"><span data-stu-id="0f046-1351">No</span></span>                               | <span data-ttu-id="0f046-1352">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-1352">Yes</span></span>                 |
| <span data-ttu-id="0f046-1353">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="0f046-1353">**Unicode**</span></span>         | <span data-ttu-id="0f046-1354">Gibt an, ob der Eigenschaftswert als Unicode gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-1354">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="0f046-1355">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="0f046-1355">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="0f046-1356">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-1356">Yes</span></span>                              | <span data-ttu-id="0f046-1357">Ja</span><span class="sxs-lookup"><span data-stu-id="0f046-1357">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="0f046-1358">Beim Generieren einer Datenbank aus einem konzeptionellen Modell erkennt der Assistent zum Generieren von Datenbanken den Wert des **StoreGeneratedPattern** -Attributs für ein **Eigenschafts** Element, wenn er sich im folgenden Namespace befindet: https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span><span class="sxs-lookup"><span data-stu-id="0f046-1358">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="0f046-1359">Die unterstützten Werte für das Attribut sind **Identity** und **berechnete**Werte.</span><span class="sxs-lookup"><span data-stu-id="0f046-1359">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="0f046-1360">Bei **einem Identitäts Wert wird eine** Daten Bank Spalte mit einem Identitäts Wert erstellt, der in der Datenbank generiert wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-1360">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="0f046-1361">Der **berechnete** Wert erstellt eine Spalte mit einem Wert, der in der Datenbank berechnet wird.</span><span class="sxs-lookup"><span data-stu-id="0f046-1361">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="0f046-1362">Beispiel</span><span class="sxs-lookup"><span data-stu-id="0f046-1362">Example</span></span>

<span data-ttu-id="0f046-1363">Das folgende Beispiel zeigt für die Eigenschaften eines Entitätstyps übernommene Facets:</span><span class="sxs-lookup"><span data-stu-id="0f046-1363">The following example shows facets applied to the properties of an entity type:</span></span>

``` xml
 <EntityType Name="Product">
   <Key>
     <PropertyRef Name="ProductId" />
   </Key>
   <Property Type="Int32"
             Name="ProductId" Nullable="false"
             a:StoreGeneratedPattern="Identity"
    xmlns:a="https://schemas.microsoft.com/ado/2009/02/edm/annotation" />
   <Property Type="String"
             Name="ProductName"
             Nullable="false"
             MaxLength="50" />
   <Property Type="String"
             Name="Location"
             Nullable="true"
             MaxLength="25" />
 </EntityType>
```
