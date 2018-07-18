---
title: CSDL-Spezifikation - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
caps.latest.revision: 3
ms.openlocfilehash: 0ece73a19fe7ea244905bccb728ab2a104c5179f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121207"
---
# <a name="csdl-specification"></a><span data-ttu-id="f7235-102">CSDL-Spezifikation</span><span class="sxs-lookup"><span data-stu-id="f7235-102">CSDL Specification</span></span>
<span data-ttu-id="f7235-103">Die konzeptionelle Schemadefinitionssprache (CSDL) ist eine XML-basierte Sprache, die die Entitäten, Beziehungen und Funktionen beschreibt, die ein konzeptionelles Modell einer datengesteuerten Anwendung bilden.</span><span class="sxs-lookup"><span data-stu-id="f7235-103">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="f7235-104">Dieses konzeptionelle Modell kann vom Entity Framework oder WCF Data Services verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="f7235-104">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="f7235-105">Die Metadaten, die in CSDL beschrieben werden wird von Entity Framework zum Zuordnen von Entitäten und Beziehungen, die in einem konzeptionellen Modell an eine Datenquelle definiert sind.</span><span class="sxs-lookup"><span data-stu-id="f7235-105">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="f7235-106">Weitere Informationen finden Sie unter [SSDL-Spezifikation](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) und [MSL-Spezifikation](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="f7235-106">For more information, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) and [MSL Specification](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).</span></span>

<span data-ttu-id="f7235-107">CSDL ist Entity Framework Implementierung des Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="f7235-107">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="f7235-108">In einer Entity Framework-Anwendung Metadaten des konzeptionellen Modells aus einer CSDL-Datei (geschrieben in CSDL) in eine Instanz von der System.Data.Metadata.Edm.EdmItemCollection geladen wird und kann mit Verwendung von Methoden in der System.Data.Metadata.Edm.MetadataWorkspace-Klasse.</span><span class="sxs-lookup"><span data-stu-id="f7235-108">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="f7235-109">Entitätsframework verwendet Metadaten des konzeptionellen Modells, um Abfragen für das konzeptionelle Modell in datenquellenspezifische Befehle zu übersetzen.</span><span class="sxs-lookup"><span data-stu-id="f7235-109">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="f7235-110">Dem EF Designer speichert Informationen zum konzeptionellen Modell in einer EDMX-Datei zur Entwurfszeit.</span><span class="sxs-lookup"><span data-stu-id="f7235-110">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="f7235-111">Zur Buildzeit verwendet dem EF Designer Informationen in einer EDMX-Datei aus, um die CSDL-Datei zu erstellen, die zur Laufzeit vom Entity Framework benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-111">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="f7235-112">Die verschiedenen Versionen von CSDL werden von XML-Namespaces unterschieden.</span><span class="sxs-lookup"><span data-stu-id="f7235-112">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="f7235-113">CSDL-Version</span><span class="sxs-lookup"><span data-stu-id="f7235-113">CSDL Version</span></span> | <span data-ttu-id="f7235-114">XML-Namespace</span><span class="sxs-lookup"><span data-stu-id="f7235-114">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="f7235-115">CSDL v1</span><span class="sxs-lookup"><span data-stu-id="f7235-115">CSDL v1</span></span>      | http://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="f7235-116">CSDL-v2</span><span class="sxs-lookup"><span data-stu-id="f7235-116">CSDL v2</span></span>      | http://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="f7235-117">CSDL v3</span><span class="sxs-lookup"><span data-stu-id="f7235-117">CSDL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="f7235-118">Zuordnungselement (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-118">Association Element (CSDL)</span></span>

<span data-ttu-id="f7235-119">Ein **Zuordnung** -Element definiert eine Beziehung zwischen zwei Entitätstypen.</span><span class="sxs-lookup"><span data-stu-id="f7235-119">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="f7235-120">Eine Zuordnung muss die Entitätstypen, die in der Beziehung enthalten sind, und die mögliche Anzahl von Entitätstypen an den Enden der Beziehung angeben, die auch als Multiplizität bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-120">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="f7235-121">Die Multiplizität eines zuordnungsendes kann einen Wert von eins (1), NULL oder eins (0.. 1) oder viele haben (\*).</span><span class="sxs-lookup"><span data-stu-id="f7235-121">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="f7235-122">Diese Informationen sind in zwei untergeordnete Ende Elemente angegeben.</span><span class="sxs-lookup"><span data-stu-id="f7235-122">This information is specified in two child End elements.</span></span>

<span data-ttu-id="f7235-123">Auf Entitätstypinstanzen an einem Ende einer Zuordnung kann über Navigationseigenschaften oder Fremdschlüssel zugegriffen werden, sofern sie für einen Entitätstyp verfügbar gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="f7235-123">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="f7235-124">In einer Anwendung stellt eine Instanz einer Zuordnung eine bestimmte Zuordnung zwischen Instanzen von Entitätstypen dar.</span><span class="sxs-lookup"><span data-stu-id="f7235-124">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="f7235-125">Zuordnungsinstanzen werden logisch in einem Zuordnungssatz gruppiert.</span><span class="sxs-lookup"><span data-stu-id="f7235-125">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="f7235-126">Ein **Zuordnung** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="f7235-126">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f7235-127">Dokumentation (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="f7235-127">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="f7235-128">Ende (genau 2 Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-128">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="f7235-129">ReferentialConstraint (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="f7235-129">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="f7235-130">Anmerkungselemente (null oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-130">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-131">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-131">Applicable Attributes</span></span>

<span data-ttu-id="f7235-132">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Zuordnung** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-132">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="f7235-133">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-133">Attribute Name</span></span> | <span data-ttu-id="f7235-134">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-134">Is Required</span></span> | <span data-ttu-id="f7235-135">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-135">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="f7235-136">**Name**</span><span class="sxs-lookup"><span data-stu-id="f7235-136">**Name**</span></span>       | <span data-ttu-id="f7235-137">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-137">Yes</span></span>         | <span data-ttu-id="f7235-138">Der Name der Zuordnung.</span><span class="sxs-lookup"><span data-stu-id="f7235-138">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="f7235-139">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Zuordnung** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-139">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="f7235-140">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-140">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-141">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-141">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="f7235-142">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-142">Example</span></span>

<span data-ttu-id="f7235-143">Das folgende Beispiel zeigt eine **Zuordnung** -Element, definiert der **CustomerOrders** Zuordnung, wenn Fremdschlüssel nicht auf verfügbar gemacht wurden die **Kunden** und  **Reihenfolge** Entitätstypen.</span><span class="sxs-lookup"><span data-stu-id="f7235-143">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="f7235-144">Die **Multiplizität** Werte für die einzelnen **End** der Zuordnung angegeben, dass viele **Bestellungen** zugeordnet sein kann eine **Kunden**, aber nur ein **Kunden** zugeordnet sein kann ein **Reihenfolge**.</span><span class="sxs-lookup"><span data-stu-id="f7235-144">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="f7235-145">Darüber hinaus die **OnDelete** Element gibt an, dass alle **Bestellungen** , beziehen sich auf eine bestimmte **Kunden** und in den geladen wurden der ObjectContext gelöscht werden Wenn die **Kunden** wird gelöscht.</span><span class="sxs-lookup"><span data-stu-id="f7235-145">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="f7235-146">Das folgende Beispiel zeigt eine **Zuordnung** -Element, definiert der **CustomerOrders** Zuordnung, wenn der Fremdschlüssel auf verfügbar gemacht wurden die **Kunden** und  **Reihenfolge** Entitätstypen.</span><span class="sxs-lookup"><span data-stu-id="f7235-146">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="f7235-147">Wurden Fremdschlüssel verfügbar gemacht, die die Beziehung zwischen den Entitäten Verwaltung erfolgt über eine **ReferentialConstraint** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-147">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="f7235-148">Ein entsprechendes AssociationSetMapping-Element ist nicht erforderlich, um diese Zuordnung mit der Datenquelle zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="f7235-148">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

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
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="f7235-149">AssociationSet-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-149">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="f7235-150">Die **AssociationSet** -Element in konzeptioneller Schemadefinitionssprache (CSDL) ist ein logischer Container für Zuordnungsinstanzen desselben Typs.</span><span class="sxs-lookup"><span data-stu-id="f7235-150">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="f7235-151">Ein Zuordnungssatz stellt eine Definition zum Gruppieren von Zuordnungsinstanzen bereit, sodass sie einer Datenquelle zugeordnet werden können.</span><span class="sxs-lookup"><span data-stu-id="f7235-151">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="f7235-152">Die **AssociationSet** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="f7235-152">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f7235-153">Dokumentation (0 (null) oder ein Element zugelassen)</span><span class="sxs-lookup"><span data-stu-id="f7235-153">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="f7235-154">Ende (genau zwei Elemente erforderlich)</span><span class="sxs-lookup"><span data-stu-id="f7235-154">End (exactly two elements required)</span></span>
-   <span data-ttu-id="f7235-155">Anmerkungselemente (0 (null) oder mehrere Elemente zugelassen)</span><span class="sxs-lookup"><span data-stu-id="f7235-155">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="f7235-156">Die **Zuordnung** Attribut gibt den Typ der Zuordnung, die ein Zuordnungssatz enthält.</span><span class="sxs-lookup"><span data-stu-id="f7235-156">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="f7235-157">Die Entitätenmengen, die die Enden eines Zuordnungssatzes bilden angegeben sind, genau mit zwei untergeordneten **End** Elemente.</span><span class="sxs-lookup"><span data-stu-id="f7235-157">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-158">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-158">Applicable Attributes</span></span>

<span data-ttu-id="f7235-159">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **AssociationSet** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-159">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="f7235-160">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-160">Attribute Name</span></span>  | <span data-ttu-id="f7235-161">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-161">Is Required</span></span> | <span data-ttu-id="f7235-162">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-162">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f7235-163">**Name**</span><span class="sxs-lookup"><span data-stu-id="f7235-163">**Name**</span></span>        | <span data-ttu-id="f7235-164">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-164">Yes</span></span>         | <span data-ttu-id="f7235-165">Der Name des Entitätssatzes.</span><span class="sxs-lookup"><span data-stu-id="f7235-165">The name of the entity set.</span></span> <span data-ttu-id="f7235-166">Der Wert des der **Namen** Attribut darf nicht den Wert der identisch sein der **Zuordnung** Attribut.</span><span class="sxs-lookup"><span data-stu-id="f7235-166">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="f7235-167">**Zuordnung**</span><span class="sxs-lookup"><span data-stu-id="f7235-167">**Association**</span></span> | <span data-ttu-id="f7235-168">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-168">Yes</span></span>         | <span data-ttu-id="f7235-169">Der vollqualifizierte Name der Zuordnung, von der der Zuordnungssatz Instanzen enthält.</span><span class="sxs-lookup"><span data-stu-id="f7235-169">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="f7235-170">Die Zuordnung muss sich im gleichen Namespace wie der Zuordnungssatz befinden.</span><span class="sxs-lookup"><span data-stu-id="f7235-170">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="f7235-171">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **AssociationSet** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-171">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="f7235-172">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-172">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-173">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-173">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="f7235-174">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-174">Example</span></span>

<span data-ttu-id="f7235-175">Das folgende Beispiel zeigt eine **EntityContainer** -Element mit zwei **AssociationSet** Elemente:</span><span class="sxs-lookup"><span data-stu-id="f7235-175">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

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
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="f7235-176">CollectionType-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-176">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="f7235-177">Die **CollectionType** -Element in konzeptioneller Schemadefinitionssprache (CSDL) gibt an, dass ein Funktionsparameter oder Funktion Rückgabetyp ist eine Sammlung.</span><span class="sxs-lookup"><span data-stu-id="f7235-177">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="f7235-178">Die **CollectionType** Element kann ein untergeordnetes Element des Parameter-Element oder das Element ReturnType (Funktion) sein.</span><span class="sxs-lookup"><span data-stu-id="f7235-178">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="f7235-179">Der Typ der Auflistung kann angegeben werden, indem Sie entweder die **Typ** Attribut oder eine der folgenden untergeordneten Elemente:</span><span class="sxs-lookup"><span data-stu-id="f7235-179">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="f7235-180">**CollectionType**</span><span class="sxs-lookup"><span data-stu-id="f7235-180">**CollectionType**</span></span>
-   <span data-ttu-id="f7235-181">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="f7235-181">ReferenceType</span></span>
-   <span data-ttu-id="f7235-182">RowType</span><span class="sxs-lookup"><span data-stu-id="f7235-182">RowType</span></span>
-   <span data-ttu-id="f7235-183">TypeRef</span><span class="sxs-lookup"><span data-stu-id="f7235-183">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="f7235-184">Ein Modell überprüft nicht, ob der Typ einer Auflistung mit den beiden angegeben ist die **Typ** -Attribut und einem untergeordneten Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-184">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-185">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-185">Applicable Attributes</span></span>

<span data-ttu-id="f7235-186">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **CollectionType** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-186">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="f7235-187">Beachten Sie, dass die **DefaultValue**, **MaxLength**, **FixedLength**, **Genauigkeit**, **Skalierung**,  **Unicode**, und **Sortierreihenfolge** Attribute gelten nur für Auflistungen von **abzielt**.</span><span class="sxs-lookup"><span data-stu-id="f7235-187">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="f7235-188">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-188">Attribute Name</span></span>                                                          | <span data-ttu-id="f7235-189">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-189">Is Required</span></span> | <span data-ttu-id="f7235-190">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-190">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f7235-191">**Type**</span><span class="sxs-lookup"><span data-stu-id="f7235-191">**Type**</span></span>                                                                | <span data-ttu-id="f7235-192">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-192">No</span></span>          | <span data-ttu-id="f7235-193">Der Typ der Auflistung.</span><span class="sxs-lookup"><span data-stu-id="f7235-193">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="f7235-194">**NULL-Werte zulässt**</span><span class="sxs-lookup"><span data-stu-id="f7235-194">**Nullable**</span></span>                                                            | <span data-ttu-id="f7235-195">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-195">No</span></span>          | <span data-ttu-id="f7235-196">**"True"** (Standardwert) oder **"false"** abhängig davon, ob die Eigenschaft einen null-Wert haben kann.</span><span class="sxs-lookup"><span data-stu-id="f7235-196">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="f7235-197">> In der CSDL-Version 1, eine Eigenschaft eines komplexen Typs benötigen `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="f7235-197">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="f7235-198">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="f7235-198">**DefaultValue**</span></span>                                                        | <span data-ttu-id="f7235-199">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-199">No</span></span>          | <span data-ttu-id="f7235-200">Der Standardwert der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="f7235-200">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="f7235-201">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="f7235-201">**MaxLength**</span></span>                                                           | <span data-ttu-id="f7235-202">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-202">No</span></span>          | <span data-ttu-id="f7235-203">Die maximale Länge des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="f7235-203">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="f7235-204">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="f7235-204">**FixedLength**</span></span>                                                         | <span data-ttu-id="f7235-205">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-205">No</span></span>          | <span data-ttu-id="f7235-206">**"True"** oder **"false"** abhängig davon, ob der Eigenschaftswert als Zeichenfolge mit fester Länge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-206">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="f7235-207">**Genauigkeit**</span><span class="sxs-lookup"><span data-stu-id="f7235-207">**Precision**</span></span>                                                           | <span data-ttu-id="f7235-208">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-208">No</span></span>          | <span data-ttu-id="f7235-209">Die Genauigkeit des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="f7235-209">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="f7235-210">**Scale** (Skalieren)</span><span class="sxs-lookup"><span data-stu-id="f7235-210">**Scale**</span></span>                                                               | <span data-ttu-id="f7235-211">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-211">No</span></span>          | <span data-ttu-id="f7235-212">Die Skalierung des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="f7235-212">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="f7235-213">**SRID**</span><span class="sxs-lookup"><span data-stu-id="f7235-213">**SRID**</span></span>                                                                | <span data-ttu-id="f7235-214">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-214">No</span></span>          | <span data-ttu-id="f7235-215">Räumliche Verweis Systembezeichner.</span><span class="sxs-lookup"><span data-stu-id="f7235-215">Spatial System Reference Identifier.</span></span> <span data-ttu-id="f7235-216">Gilt nur für Eigenschaften des räumlichen Typen.</span><span class="sxs-lookup"><span data-stu-id="f7235-216">Valid only for properties of spatial types.</span></span>   <span data-ttu-id="f7235-217">Weitere Informationen finden Sie unter [SRID](http://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span><span class="sxs-lookup"><span data-stu-id="f7235-217">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="f7235-218">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="f7235-218">**Unicode**</span></span>                                                             | <span data-ttu-id="f7235-219">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-219">No</span></span>          | <span data-ttu-id="f7235-220">**"True"** oder **"false"** abhängig davon, ob der Eigenschaftswert als Unicode-Zeichenfolge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-220">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="f7235-221">**Sortierung**</span><span class="sxs-lookup"><span data-stu-id="f7235-221">**Collation**</span></span>                                                           | <span data-ttu-id="f7235-222">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-222">No</span></span>          | <span data-ttu-id="f7235-223">Eine Zeichenfolge, die die Sortierreihenfolge angibt, die in der Datenquelle verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="f7235-223">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="f7235-224">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **CollectionType** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-224">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="f7235-225">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-226">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="f7235-227">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-227">Example</span></span>

<span data-ttu-id="f7235-228">Das folgende Beispiel zeigt eine modelldefinierte Funktion, die mithilfe, einer **CollectionType** Element, um anzugeben, dass die Funktion, eine Auflistung von zurückgibt **Person** Entitätstypen (gemäß der Angabe der **ElementType** Attribut).</span><span class="sxs-lookup"><span data-stu-id="f7235-228">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

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
 

<span data-ttu-id="f7235-229">Das folgende Beispiel zeigt eine modelldefinierte Funktion, verwendet eine **CollectionType** Element, um anzugeben, dass die Funktion eine Auflistung von Zeilen zurückgibt (gemäß den Angaben in der **RowType** Element).</span><span class="sxs-lookup"><span data-stu-id="f7235-229">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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
 

<span data-ttu-id="f7235-230">Das folgende Beispiel zeigt eine modelldefinierte Funktion, verwendet der **CollectionType** Element, um anzugeben, dass die Funktion als Parameter, eine Auflistung von akzeptiert **Abteilung** Entitätstypen.</span><span class="sxs-lookup"><span data-stu-id="f7235-230">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

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
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="f7235-231">ComplexType-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-231">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="f7235-232">Ein **ComplexType** -Element definiert eine Datenstruktur, bestehend aus **EdmSimpleType** Eigenschaften oder anderen komplexen Typen.</span><span class="sxs-lookup"><span data-stu-id="f7235-232">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span>  <span data-ttu-id="f7235-233">Ein komplexer Typ kann eine Eigenschaft eines Entitätstyps oder eines anderen komplexen Typs sein.</span><span class="sxs-lookup"><span data-stu-id="f7235-233">A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="f7235-234">Ein komplexer Typ entspricht einem Entitätstyp, in dem von einem komplexen Typ Daten definiert werden.</span><span class="sxs-lookup"><span data-stu-id="f7235-234">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="f7235-235">Es gibt jedoch einige Hauptunterschiede zwischen komplexen Typen und Entitätstypen:</span><span class="sxs-lookup"><span data-stu-id="f7235-235">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="f7235-236">Komplexe Typen weisen keine Identitäten (oder Schlüssel) auf und können daher nicht unabhängig sein.</span><span class="sxs-lookup"><span data-stu-id="f7235-236">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="f7235-237">Komplexe Typen können nur Eigenschaften von Entitätstypen oder anderen komplexen Typen sein.</span><span class="sxs-lookup"><span data-stu-id="f7235-237">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="f7235-238">Komplexe Typen können nicht an Zuordnungen beteiligt sein.</span><span class="sxs-lookup"><span data-stu-id="f7235-238">Complex types cannot participate in associations.</span></span> <span data-ttu-id="f7235-239">Weder Ende einer Zuordnung kann ein komplexer Typ sein, und daher Navigationseigenschaften können nicht für komplexe Typen definiert werden.</span><span class="sxs-lookup"><span data-stu-id="f7235-239">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="f7235-240">Einer komplexen Typeigenschaft kann kein NULL-Wert zugewiesen werden, obwohl jede skalare Eigenschaft eines komplexen Typs auf NULL festgelegt werden kann.</span><span class="sxs-lookup"><span data-stu-id="f7235-240">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="f7235-241">Ein **ComplexType** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="f7235-241">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f7235-242">Dokumentation (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="f7235-242">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="f7235-243">Eigenschaft (null oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-243">Property (zero or more elements)</span></span>
-   <span data-ttu-id="f7235-244">Anmerkungselemente (null oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-244">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="f7235-245">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **ComplexType** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-245">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="f7235-246">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-246">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="f7235-247">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-247">Is Required</span></span> | <span data-ttu-id="f7235-248">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-248">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f7235-249">name</span><span class="sxs-lookup"><span data-stu-id="f7235-249">Name</span></span>                                                                                                           | <span data-ttu-id="f7235-250">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-250">Yes</span></span>         | <span data-ttu-id="f7235-251">Der Name des komplexen Typs.</span><span class="sxs-lookup"><span data-stu-id="f7235-251">The name of the complex type.</span></span> <span data-ttu-id="f7235-252">Der Name eines komplexen Typs darf nicht dem Namen anderer komplexer Typen, Entitätstypen oder Zuordnungen entsprechen, die sich innerhalb des Bereichs des Modells befinden.</span><span class="sxs-lookup"><span data-stu-id="f7235-252">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="f7235-253">BaseType</span><span class="sxs-lookup"><span data-stu-id="f7235-253">BaseType</span></span>                                                                                                       | <span data-ttu-id="f7235-254">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-254">No</span></span>          | <span data-ttu-id="f7235-255">Der Name eines anderen komplexen Typs, der der Basistyp des zu definierenden komplexen Typs ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-255">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="f7235-256">> Dieses Attribut ist nicht anwendbar, in der CSDL-v1.</span><span class="sxs-lookup"><span data-stu-id="f7235-256">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="f7235-257">Für komplexe Typen wird Vererbung in dieser Version nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="f7235-257">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="f7235-258">Abstrakt</span><span class="sxs-lookup"><span data-stu-id="f7235-258">Abstract</span></span>                                                                                                       | <span data-ttu-id="f7235-259">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-259">No</span></span>          | <span data-ttu-id="f7235-260">**"True"** oder **"false"** (Standardwert) abhängig davon, ob der komplexe Typ ein abstrakter Typ.</span><span class="sxs-lookup"><span data-stu-id="f7235-260">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="f7235-261">> Dieses Attribut ist nicht anwendbar, in der CSDL-v1.</span><span class="sxs-lookup"><span data-stu-id="f7235-261">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="f7235-262">Komplexe Typen in dieser Version können keine abstrakten Typen sein.</span><span class="sxs-lookup"><span data-stu-id="f7235-262">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="f7235-263">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **ComplexType** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-263">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="f7235-264">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-264">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-265">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-265">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="f7235-266">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-266">Example</span></span>

<span data-ttu-id="f7235-267">Das folgende Beispiel zeigt einen komplexen Typ **Adresse**, mit der **EdmSimpleType** Eigenschaften **StreetAddress**, **City**,  **StateOrProvince**, **Land**, und **PostalCode**.</span><span class="sxs-lookup"><span data-stu-id="f7235-267">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="f7235-268">Um den komplexen Typ definieren **Adresse** (siehe oben) als Eigenschaft eines Entitätstyps handelt, müssen Sie deklarieren den Eigenschaftentyp in der Definition des Entitätstyps.</span><span class="sxs-lookup"><span data-stu-id="f7235-268">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="f7235-269">Das folgende Beispiel zeigt die **Adresse** Eigenschaft als komplexen Typ für einen Entitätstyp (**Verleger**):</span><span class="sxs-lookup"><span data-stu-id="f7235-269">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

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
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="f7235-270">DefiningExpression-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-270">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="f7235-271">Die **DefiningExpression** -Element in konzeptioneller Schemadefinitionssprache (CSDL) enthält einen Entity SQL-Ausdruck, der eine Funktion im konzeptionellen Modell definiert.</span><span class="sxs-lookup"><span data-stu-id="f7235-271">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="f7235-272">Zu Validierungszwecken kann ein **DefiningExpression** -Element beliebigen Inhalt enthalten.</span><span class="sxs-lookup"><span data-stu-id="f7235-272">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="f7235-273">Allerdings Entity Framework löst eine Ausnahme zur Laufzeit Wenn eine **DefiningExpression** Element enthält keinen gültigen Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="f7235-273">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-274">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-274">Applicable Attributes</span></span>

<span data-ttu-id="f7235-275">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **DefiningExpression** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-275">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="f7235-276">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-276">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-277">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-277">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="f7235-278">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-278">Example</span></span>

<span data-ttu-id="f7235-279">Im folgenden Beispiel wird eine **DefiningExpression** Element eine Funktion definiert, die die Anzahl der Jahre zurückgibt, da ein Buch veröffentlicht wurde.</span><span class="sxs-lookup"><span data-stu-id="f7235-279">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="f7235-280">Den Inhalt der **DefiningExpression** Element ist in Entity SQL geschrieben.</span><span class="sxs-lookup"><span data-stu-id="f7235-280">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="f7235-281">Dependent-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-281">Dependent Element (CSDL)</span></span>

<span data-ttu-id="f7235-282">Die **abhängige** -Element in konzeptioneller Schemadefinitionssprache (CSDL) ist ein untergeordnetes Element auf das ReferentialConstraint-Element und definiert das abhängige Ende einer referenziellen Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="f7235-282">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="f7235-283">Ein **ReferentialConstraint** -Element definiert Funktionen, die eine Einschränkung der referenziellen Integrität in einer relationalen Datenbank ähnlich ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-283">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="f7235-284">So wie eine Spalte (oder Spalten) einer Datenbanktabelle auf den Primärschlüssel einer anderen Tabelle verweisen kann, kann eine Eigenschaft (oder Eigenschaften) eines Entitätstyps auf den Entitätsschlüssel eines anderen Entitätstyps verweisen.</span><span class="sxs-lookup"><span data-stu-id="f7235-284">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="f7235-285">Wird aufgerufen, der Entitätstyp, auf die verwiesen wird, wird die *prinzipalende* der Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="f7235-285">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="f7235-286">Der Entitätstyp, der auf das prinzipalende verweist heißt die *abhängigen Endes* der Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="f7235-286">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="f7235-287">**PropertyRef** Elemente werden verwendet, um anzugeben, welche Schlüssel auf das prinzipalende verweisen.</span><span class="sxs-lookup"><span data-stu-id="f7235-287">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="f7235-288">Die **abhängige** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="f7235-288">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f7235-289">PropertyRef (eine oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-289">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="f7235-290">Anmerkungselemente (null oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-290">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-291">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-291">Applicable Attributes</span></span>

<span data-ttu-id="f7235-292">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **abhängige** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-292">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="f7235-293">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-293">Attribute Name</span></span> | <span data-ttu-id="f7235-294">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-294">Is Required</span></span> | <span data-ttu-id="f7235-295">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-295">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="f7235-296">**Rolle**</span><span class="sxs-lookup"><span data-stu-id="f7235-296">**Role**</span></span>       | <span data-ttu-id="f7235-297">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-297">Yes</span></span>         | <span data-ttu-id="f7235-298">Der Name des Entitätstyps am abhängigen Ende der Zuordnung.</span><span class="sxs-lookup"><span data-stu-id="f7235-298">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="f7235-299">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **abhängige** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-299">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="f7235-300">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-300">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-301">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-301">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="f7235-302">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-302">Example</span></span>

<span data-ttu-id="f7235-303">Das folgende Beispiel zeigt eine **ReferentialConstraint** Element verwendet wird, als Teil der Definition der **PublishedBy** Zuordnung.</span><span class="sxs-lookup"><span data-stu-id="f7235-303">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="f7235-304">Die **PublisherId** Eigenschaft der **Buch** -Entitätstyps bildet das abhängige Ende der referenziellen Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="f7235-304">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

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
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="f7235-305">Documentation-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-305">Documentation Element (CSDL)</span></span>

<span data-ttu-id="f7235-306">Die **Dokumentation** -Element in konzeptioneller Schemadefinitionssprache (CSDL) kann verwendet werden, um Informationen zu einem Objekt bereitzustellen, die in einem übergeordneten Element definiert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-306">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="f7235-307">In einer EDMX-Datei Wenn die **Dokumentation** Element ist ein untergeordnetes Element eines Elements, das als ein Objekt auf der Entwurfsoberfläche des EF-Designers (z. B. eine Entität, Zuordnung oder Eigenschaft), den Inhalt der angezeigt wird der **Dokumentation**  Element wird angezeigt, in der Visual Studio **Eigenschaften** Fenster für das Objekt.</span><span class="sxs-lookup"><span data-stu-id="f7235-307">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="f7235-308">Die **Dokumentation** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="f7235-308">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f7235-309">**Zusammenfassung**: eine kurze Beschreibung des übergeordneten Elements.</span><span class="sxs-lookup"><span data-stu-id="f7235-309">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="f7235-310">(kein (Null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="f7235-310">(zero or one element)</span></span>
-   <span data-ttu-id="f7235-311">**LongDescription**: eine ausführliche Beschreibung des übergeordneten Elements.</span><span class="sxs-lookup"><span data-stu-id="f7235-311">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="f7235-312">(kein (Null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="f7235-312">(zero or one element)</span></span>
-   <span data-ttu-id="f7235-313">Anmerkungselemente.</span><span class="sxs-lookup"><span data-stu-id="f7235-313">Annotation elements.</span></span> <span data-ttu-id="f7235-314">(keine (Null) oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-314">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-315">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-315">Applicable Attributes</span></span>

<span data-ttu-id="f7235-316">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Dokumentation** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-316">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="f7235-317">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-317">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-318">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-318">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="f7235-319">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-319">Example</span></span>

<span data-ttu-id="f7235-320">Das folgende Beispiel zeigt die **Dokumentation** Element als untergeordnetes Element eines EntityType-Elements.</span><span class="sxs-lookup"><span data-stu-id="f7235-320">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="f7235-321">Wäre im Codeausschnitt weiter unten in der CSDL Inhalt einer EDMX-Datei, den Inhalt der **Zusammenfassung** und **LongDescription** Elemente angezeigt werden würden, in der Visual Studio **Eigenschaften** Fenster beim Klicken auf die `Customer` Entitätstyp.</span><span class="sxs-lookup"><span data-stu-id="f7235-321">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

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
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="f7235-322">End-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-322">End Element (CSDL)</span></span>

<span data-ttu-id="f7235-323">Die **End** -Element in konzeptioneller Schemadefinitionssprache (CSDL) kann ein untergeordnetes Element des Association-Element oder das AssociationSet-Element sein.</span><span class="sxs-lookup"><span data-stu-id="f7235-323">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="f7235-324">In jedem Fall die Rolle der **End** Elements und die anwendbaren Attribute unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="f7235-324">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="f7235-325">Das End-Element als untergeordnetes Objekt des Association-Elements</span><span class="sxs-lookup"><span data-stu-id="f7235-325">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="f7235-326">Ein **End** Element (als untergeordnetes Element der **Zuordnung** Element) gibt den Entitätstyp auf ein Ende einer Zuordnung und die Anzahl der Instanzen eines Entitätstyps, die an diesem Ende einer Zuordnung vorhanden sein können.</span><span class="sxs-lookup"><span data-stu-id="f7235-326">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="f7235-327">Zuordnungsenden werden als Teil einer Zuordnung definiert. Eine Zuordnung muss genau zwei Zuordnungsenden aufweisen.</span><span class="sxs-lookup"><span data-stu-id="f7235-327">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="f7235-328">Auf Entitätstypinstanzen an einem Ende einer Zuordnung kann über Navigationseigenschaften oder Fremdschlüssel zugegriffen werden, sofern sie für einen Entitätstyp verfügbar gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="f7235-328">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="f7235-329">Ein **End** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="f7235-329">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f7235-330">Dokumentation (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="f7235-330">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="f7235-331">OnDelete (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="f7235-331">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="f7235-332">Anmerkungselemente (null oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-332">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="f7235-333">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-333">Applicable Attributes</span></span>

<span data-ttu-id="f7235-334">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **End** -Element, wenn es sich um das untergeordnete Element ist ein **Zuordnung** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-334">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="f7235-335">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-335">Attribute Name</span></span>   | <span data-ttu-id="f7235-336">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-336">Is Required</span></span> | <span data-ttu-id="f7235-337">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-337">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f7235-338">**Type**</span><span class="sxs-lookup"><span data-stu-id="f7235-338">**Type**</span></span>         | <span data-ttu-id="f7235-339">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-339">Yes</span></span>         | <span data-ttu-id="f7235-340">Der Name des Entitätstyps an einem Ende der Zuordnung.</span><span class="sxs-lookup"><span data-stu-id="f7235-340">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="f7235-341">**Rolle**</span><span class="sxs-lookup"><span data-stu-id="f7235-341">**Role**</span></span>         | <span data-ttu-id="f7235-342">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-342">No</span></span>          | <span data-ttu-id="f7235-343">Der Name für das Zuordnungsende.</span><span class="sxs-lookup"><span data-stu-id="f7235-343">A name for the association end.</span></span> <span data-ttu-id="f7235-344">Wird kein Name angegeben, wird der Name des Entitätstyps am Zuordnungsende verwendet.</span><span class="sxs-lookup"><span data-stu-id="f7235-344">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="f7235-345">**Multiplizität**</span><span class="sxs-lookup"><span data-stu-id="f7235-345">**Multiplicity**</span></span> | <span data-ttu-id="f7235-346">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-346">Yes</span></span>         | <span data-ttu-id="f7235-347">**1**, **0.. 1**, oder **\*** abhängig von der Anzahl der Entitätstypinstanzen, die am Ende der Zuordnung sein können.</span><span class="sxs-lookup"><span data-stu-id="f7235-347">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="f7235-348">**1** gibt an, genau eine Entitätstypinstanz am Zuordnungsende vorhanden.</span><span class="sxs-lookup"><span data-stu-id="f7235-348">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="f7235-349">**0.. 1** gibt an, dass 0 (null) oder eine Entitätstypinstanz am Zuordnungsende vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-349">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="f7235-350">**\*** Gibt an, dass 0 (null), einer oder mehrere Instanzen eines Entitätstyps am Zuordnungsende vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="f7235-350">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="f7235-351">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **End** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-351">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="f7235-352">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-352">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-353">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-353">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="f7235-354">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-354">Example</span></span>

<span data-ttu-id="f7235-355">Das folgende Beispiel zeigt eine **Zuordnung** -Element, definiert der **CustomerOrders** Zuordnung.</span><span class="sxs-lookup"><span data-stu-id="f7235-355">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="f7235-356">Die **Multiplizität** Werte für die einzelnen **End** der Zuordnung angegeben, dass viele **Bestellungen** zugeordnet sein kann eine **Kunden**, aber nur ein **Kunden** zugeordnet sein kann ein **Reihenfolge**.</span><span class="sxs-lookup"><span data-stu-id="f7235-356">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="f7235-357">Darüber hinaus die **OnDelete** Element gibt an, dass alle **Bestellungen** , beziehen sich auf eine bestimmte **Kunden** und in geladen wurden ObjectContext werden Gelöschte If der **Kunden** wird gelöscht.</span><span class="sxs-lookup"><span data-stu-id="f7235-357">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="f7235-358">Das End-Element als untergeordnetes Objekt des AssociationSet-Elements</span><span class="sxs-lookup"><span data-stu-id="f7235-358">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="f7235-359">Die **End** -Element gibt ein Ende des Zuordnungssatzes.</span><span class="sxs-lookup"><span data-stu-id="f7235-359">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="f7235-360">Die **AssociationSet** -Element muss enthalten zwei **End** Elemente.</span><span class="sxs-lookup"><span data-stu-id="f7235-360">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="f7235-361">Die Informationen in einer **End** Element wird beim Zuordnen eines Zuordnungssatzes zu einer Datenquelle verwendet.</span><span class="sxs-lookup"><span data-stu-id="f7235-361">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="f7235-362">Ein **End** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="f7235-362">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f7235-363">Dokumentation (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="f7235-363">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="f7235-364">Anmerkungselemente (null oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-364">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="f7235-365">Anmerkungselemente müssen an alle anderen untergeordneten Elemente angereiht werden.</span><span class="sxs-lookup"><span data-stu-id="f7235-365">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="f7235-366">Anmerkungselemente sind nur zulässig, in der CSDL-v2 und höher.</span><span class="sxs-lookup"><span data-stu-id="f7235-366">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="f7235-367">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-367">Applicable Attributes</span></span>

<span data-ttu-id="f7235-368">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **End** -Element, wenn es sich um das untergeordnete Element ist ein **AssociationSet** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-368">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="f7235-369">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-369">Attribute Name</span></span> | <span data-ttu-id="f7235-370">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-370">Is Required</span></span> | <span data-ttu-id="f7235-371">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-371">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f7235-372">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="f7235-372">**EntitySet**</span></span>  | <span data-ttu-id="f7235-373">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-373">Yes</span></span>         | <span data-ttu-id="f7235-374">Der Name des der **EntitySet** Element, das ein Ende des übergeordneten Elements definiert **AssociationSet** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-374">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="f7235-375">Die **EntitySet** Element definiert werden muss, in der gleichen Entitätscontainer wie das übergeordnete Element **AssociationSet** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-375">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="f7235-376">**Rolle**</span><span class="sxs-lookup"><span data-stu-id="f7235-376">**Role**</span></span>       | <span data-ttu-id="f7235-377">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-377">No</span></span>          | <span data-ttu-id="f7235-378">Der Name des Endes des Zuordnungssatzes.</span><span class="sxs-lookup"><span data-stu-id="f7235-378">The name of the association set end.</span></span> <span data-ttu-id="f7235-379">Wenn die **Rolle** Attribut nicht verwendet wird, der Name des zuordnungssatzendes wird der Name der Entitätenmenge sein.</span><span class="sxs-lookup"><span data-stu-id="f7235-379">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="f7235-380">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **End** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-380">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="f7235-381">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-381">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-382">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-382">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="f7235-383">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-383">Example</span></span>

<span data-ttu-id="f7235-384">Das folgende Beispiel zeigt eine **EntityContainer** -Element mit zwei **AssociationSet** Elementen, die jeweils mit zwei **End** Elemente:</span><span class="sxs-lookup"><span data-stu-id="f7235-384">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

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
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="f7235-385">EntityContainer-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-385">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="f7235-386">Die **EntityContainer** -Element in konzeptioneller Schemadefinitionssprache (CSDL) ist ein logischer Container für Entitätssätze, Zuordnungssätze und Funktionsimporte.</span><span class="sxs-lookup"><span data-stu-id="f7235-386">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="f7235-387">Ordnet ein Entitätscontainer eines konzeptionellen Modells ein Speichermodell-Entitätscontainer über die EntityContainerMapping-Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-387">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="f7235-388">Ein Speichermodell-Entitätscontainer beschreibt die Struktur der Datenbank: Entitätssätze beschreiben Tabellen, Zuordnungssätze beschreiben Fremdschlüsseleinschränkungen, und Funktionsimporte beschreiben gespeicherte Prozeduren in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="f7235-388">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="f7235-389">Ein **EntityContainer** -Element kann NULL oder eine dokumentationselemente aufweisen.</span><span class="sxs-lookup"><span data-stu-id="f7235-389">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="f7235-390">Wenn eine **Dokumentation** -Element vorhanden ist, muss sich davor, alle **EntitySet**, **AssociationSet**, und **FunctionImport** Elemente.</span><span class="sxs-lookup"><span data-stu-id="f7235-390">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="f7235-391">Ein **EntityContainer** -Element kann 0 (null) oder mehrere der folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet) aufweisen:</span><span class="sxs-lookup"><span data-stu-id="f7235-391">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f7235-392">EntitySet</span><span class="sxs-lookup"><span data-stu-id="f7235-392">EntitySet</span></span>
-   <span data-ttu-id="f7235-393">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="f7235-393">AssociationSet</span></span>
-   <span data-ttu-id="f7235-394">FunctionImport</span><span class="sxs-lookup"><span data-stu-id="f7235-394">FunctionImport</span></span>
-   <span data-ttu-id="f7235-395">Anmerkungselemente</span><span class="sxs-lookup"><span data-stu-id="f7235-395">Annotation elements</span></span>

<span data-ttu-id="f7235-396">Sie können erweitern, eine **EntityContainer** zum Angeben von den Inhalt eines anderen **EntityContainer** , der sich im selben Namespace.</span><span class="sxs-lookup"><span data-stu-id="f7235-396">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="f7235-397">Um den Inhalt eines anderen einzuschließen **EntityContainer**, im verweisenden **EntityContainer** -Element legen Sie den Wert des der **erweitert** -Attribut auf den Namen der  **EntityContainer** -Element, das enthalten sein sollen.</span><span class="sxs-lookup"><span data-stu-id="f7235-397">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="f7235-398">Alle untergeordneten Elemente des eingeschlossenen **EntityContainer** Element behandelt werden als untergeordnete Elemente des verweisenden **EntityContainer** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-398">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-399">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-399">Applicable Attributes</span></span>

<span data-ttu-id="f7235-400">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Using** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-400">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="f7235-401">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-401">Attribute Name</span></span> | <span data-ttu-id="f7235-402">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-402">Is Required</span></span> | <span data-ttu-id="f7235-403">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-403">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="f7235-404">**Name**</span><span class="sxs-lookup"><span data-stu-id="f7235-404">**Name**</span></span>       | <span data-ttu-id="f7235-405">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-405">Yes</span></span>         | <span data-ttu-id="f7235-406">Der Name des Entitätscontainers.</span><span class="sxs-lookup"><span data-stu-id="f7235-406">The name of the entity container.</span></span>                               |
| <span data-ttu-id="f7235-407">**Erweitert**</span><span class="sxs-lookup"><span data-stu-id="f7235-407">**Extends**</span></span>    | <span data-ttu-id="f7235-408">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-408">No</span></span>          | <span data-ttu-id="f7235-409">Der Name eines anderen Entitätscontainers innerhalb des gleichen Namespaces.</span><span class="sxs-lookup"><span data-stu-id="f7235-409">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="f7235-410">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **EntityContainer** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-410">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="f7235-411">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-411">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-412">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-412">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="f7235-413">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-413">Example</span></span>

<span data-ttu-id="f7235-414">Das folgende Beispiel zeigt eine **EntityContainer** -Element, das drei Entitätenmengen und zwei Zuordnungssätze definiert.</span><span class="sxs-lookup"><span data-stu-id="f7235-414">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

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
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="f7235-415">EntitySet-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-415">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="f7235-416">Die **EntitySet** -Element in konzeptioneller Schemadefinitionssprache ist ein logischer Container für Instanzen eines Entitätstyps und Instanzen eines beliebigen Typs, die von diesem Entitätstyp abgeleitet ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-416">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="f7235-417">Die Beziehung zwischen einem Entitätstyp und einem Entitätssatz ist zur Beziehung zwischen einer Zeile und einer Tabelle in einer relationalen Datenbank analog.</span><span class="sxs-lookup"><span data-stu-id="f7235-417">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="f7235-418">Wie eine Zeile definiert ein Entitätstyp einen Satz verknüpfter Daten, und ebenso wie eine Tabelle enthält ein Entitätssatz Instanzen dieser Definition.</span><span class="sxs-lookup"><span data-stu-id="f7235-418">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="f7235-419">Ein Entitätssatz stellt ein Konstrukt zum Gruppieren von Entitätstypinstanzen bereit, damit diese verwandten Datenstrukturen in einer Datenquelle zugeordnet werden können.</span><span class="sxs-lookup"><span data-stu-id="f7235-419">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="f7235-420">Für einen bestimmten Entitätstyp kann mindestens ein Entitätssatz definiert werden.</span><span class="sxs-lookup"><span data-stu-id="f7235-420">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="f7235-421">Dem EF Designer unterstützt keine konzeptionelle Modellen, die mehrere Entitätenmengen pro Typ enthalten.</span><span class="sxs-lookup"><span data-stu-id="f7235-421">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="f7235-422">Die **EntitySet** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="f7235-422">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f7235-423">Documentation-Element (null oder ein Element zugelassen)</span><span class="sxs-lookup"><span data-stu-id="f7235-423">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="f7235-424">Anmerkungselemente (0 (null) oder mehrere Elemente zugelassen)</span><span class="sxs-lookup"><span data-stu-id="f7235-424">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-425">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-425">Applicable Attributes</span></span>

<span data-ttu-id="f7235-426">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **EntitySet** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-426">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="f7235-427">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-427">Attribute Name</span></span> | <span data-ttu-id="f7235-428">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-428">Is Required</span></span> | <span data-ttu-id="f7235-429">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-429">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="f7235-430">**Name**</span><span class="sxs-lookup"><span data-stu-id="f7235-430">**Name**</span></span>       | <span data-ttu-id="f7235-431">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-431">Yes</span></span>         | <span data-ttu-id="f7235-432">Der Name des Entitätssatzes.</span><span class="sxs-lookup"><span data-stu-id="f7235-432">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="f7235-433">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="f7235-433">**EntityType**</span></span> | <span data-ttu-id="f7235-434">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-434">Yes</span></span>         | <span data-ttu-id="f7235-435">Der vollqualifizierte Name des Entitätstyps, für den der Entitätssatz Instanzen enthält.</span><span class="sxs-lookup"><span data-stu-id="f7235-435">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="f7235-436">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **EntitySet** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-436">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="f7235-437">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-437">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-438">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-438">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="f7235-439">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-439">Example</span></span>

<span data-ttu-id="f7235-440">Das folgende Beispiel zeigt eine **EntityContainer** -Element mit drei **EntitySet** Elemente:</span><span class="sxs-lookup"><span data-stu-id="f7235-440">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

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
 

<span data-ttu-id="f7235-441">Pro Typ (MEST) können mehrere Entitätssätze definiert werden.</span><span class="sxs-lookup"><span data-stu-id="f7235-441">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="f7235-442">Das folgende Beispiel definiert einen Entitätscontainer mit zwei Entitätenmengen für den **Buch** Entitätstyp:</span><span class="sxs-lookup"><span data-stu-id="f7235-442">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

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
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="f7235-443">EntityType-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-443">EntityType Element (CSDL)</span></span>

<span data-ttu-id="f7235-444">Die **EntityType** -Element stellt die Struktur des Konzept der obersten Ebene, z. B. ein Kunde oder eine Bestellung in einem konzeptionellen Modell dar.</span><span class="sxs-lookup"><span data-stu-id="f7235-444">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="f7235-445">Ein Entitätstyp ist eine Vorlage für Instanzen von Entitätstypen in einer Anwendung.</span><span class="sxs-lookup"><span data-stu-id="f7235-445">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="f7235-446">Jede Vorlage enthält die folgenden Informationen:</span><span class="sxs-lookup"><span data-stu-id="f7235-446">Each template contains the following information:</span></span>

-   <span data-ttu-id="f7235-447">Eine eindeutige Bezeichnung.</span><span class="sxs-lookup"><span data-stu-id="f7235-447">A unique name.</span></span> <span data-ttu-id="f7235-448">(Erforderlich.)</span><span class="sxs-lookup"><span data-stu-id="f7235-448">(Required.)</span></span>
-   <span data-ttu-id="f7235-449">Ein Entitätsschlüssel, der von einem oder mehreren Eigenschaften definiert wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-449">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="f7235-450">(Erforderlich.)</span><span class="sxs-lookup"><span data-stu-id="f7235-450">(Required.)</span></span>
-   <span data-ttu-id="f7235-451">Eigenschaften für enthaltene Daten.</span><span class="sxs-lookup"><span data-stu-id="f7235-451">Properties for containing data.</span></span> <span data-ttu-id="f7235-452">(Optional)</span><span class="sxs-lookup"><span data-stu-id="f7235-452">(Optional.)</span></span>
-   <span data-ttu-id="f7235-453">Navigationseigenschaften, die die Navigation von einem Ende einer Zuordnung zum anderen Ende ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="f7235-453">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="f7235-454">(Optional)</span><span class="sxs-lookup"><span data-stu-id="f7235-454">(Optional.)</span></span>

<span data-ttu-id="f7235-455">In einer Anwendung stellt eine Instanz eines Entitätstyps ein spezielles Objekt dar, wie etwa einen bestimmten Kunden oder eine Bestellung.</span><span class="sxs-lookup"><span data-stu-id="f7235-455">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="f7235-456">Jede Instanz eines Entitätstyps muss über einen eindeutigen Entitätsschlüssel innerhalb eines Entitätssatzes verfügen.</span><span class="sxs-lookup"><span data-stu-id="f7235-456">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="f7235-457">Zwei Instanzen eines Entitätstyps werden nur dann als gleich betrachtet, wenn sie vom selben Typ sind und die Werte ihrer Entitätsschlüssel übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-457">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="f7235-458">Ein **EntityType** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="f7235-458">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f7235-459">Dokumentation (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="f7235-459">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="f7235-460">Schlüssel (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="f7235-460">Key (zero or one element)</span></span>
-   <span data-ttu-id="f7235-461">Eigenschaft (null oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-461">Property (zero or more elements)</span></span>
-   <span data-ttu-id="f7235-462">NavigationProperty (null oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-462">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="f7235-463">Anmerkungselemente (null oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-463">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-464">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-464">Applicable Attributes</span></span>

<span data-ttu-id="f7235-465">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **EntityType** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-465">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="f7235-466">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-466">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="f7235-467">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-467">Is Required</span></span> | <span data-ttu-id="f7235-468">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-468">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f7235-469">**Name**</span><span class="sxs-lookup"><span data-stu-id="f7235-469">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="f7235-470">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-470">Yes</span></span>         | <span data-ttu-id="f7235-471">Der Name des Entitätstyps.</span><span class="sxs-lookup"><span data-stu-id="f7235-471">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="f7235-472">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="f7235-472">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="f7235-473">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-473">No</span></span>          | <span data-ttu-id="f7235-474">Der Name eines anderen Entitätstyps, der der Basistyp des zu definierenden Entitätstyps ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-474">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="f7235-475">**Abstrakt**</span><span class="sxs-lookup"><span data-stu-id="f7235-475">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="f7235-476">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-476">No</span></span>          | <span data-ttu-id="f7235-477">**"True"** oder **"false"**, abhängig davon, ob der Entitätstyp ein abstrakter Typ ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-477">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="f7235-478">**OpenType**</span><span class="sxs-lookup"><span data-stu-id="f7235-478">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="f7235-479">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-479">No</span></span>          | <span data-ttu-id="f7235-480">**"True"** oder **"false"** abhängig davon, ob der Entitätstyp ein offener Entitätstyp ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-480">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="f7235-481">> Die **OpenType** Attribut gilt nur für Entitätstypen, die in konzeptionellen Modellen definiert werden, die mit ADO.NET Data Services verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="f7235-481">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="f7235-482">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **EntityType** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-482">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="f7235-483">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-483">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-484">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-484">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="f7235-485">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-485">Example</span></span>

<span data-ttu-id="f7235-486">Das folgende Beispiel zeigt eine **EntityType** -Element mit drei **Eigenschaft** Elementen und zwei **NavigationProperty** Elemente:</span><span class="sxs-lookup"><span data-stu-id="f7235-486">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

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
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="f7235-487">EnumType-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-487">EnumType Element (CSDL)</span></span>

<span data-ttu-id="f7235-488">Die **EnumType** Element stellt einen Enumerationstyp dar.</span><span class="sxs-lookup"><span data-stu-id="f7235-488">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="f7235-489">Ein **EnumType** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="f7235-489">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f7235-490">Dokumentation (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="f7235-490">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="f7235-491">Element (null oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-491">Member (zero or more elements)</span></span>
-   <span data-ttu-id="f7235-492">Anmerkungselemente (null oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-492">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-493">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-493">Applicable Attributes</span></span>

<span data-ttu-id="f7235-494">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **EnumType** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-494">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="f7235-495">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-495">Attribute Name</span></span>     | <span data-ttu-id="f7235-496">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-496">Is Required</span></span> | <span data-ttu-id="f7235-497">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-497">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f7235-498">**Name**</span><span class="sxs-lookup"><span data-stu-id="f7235-498">**Name**</span></span>           | <span data-ttu-id="f7235-499">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-499">Yes</span></span>         | <span data-ttu-id="f7235-500">Der Name des Entitätstyps.</span><span class="sxs-lookup"><span data-stu-id="f7235-500">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="f7235-501">**IsFlags**</span><span class="sxs-lookup"><span data-stu-id="f7235-501">**IsFlags**</span></span>        | <span data-ttu-id="f7235-502">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-502">No</span></span>          | <span data-ttu-id="f7235-503">**"True"** oder **"false"**, abhängig davon, ob der Enumerationstyp als einen Satz von Flags verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="f7235-503">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="f7235-504">Der Standardwert ist **"false".**.</span><span class="sxs-lookup"><span data-stu-id="f7235-504">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="f7235-505">**UnderlyingType**</span><span class="sxs-lookup"><span data-stu-id="f7235-505">**UnderlyingType**</span></span> | <span data-ttu-id="f7235-506">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-506">No</span></span>          | <span data-ttu-id="f7235-507">**Edm.Byte**, **Edm.Int16**, **Int32**, **Int64** oder **Edm.SByte** definieren den Bereich von Werten des Typs.</span><span class="sxs-lookup"><span data-stu-id="f7235-507">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span>   <span data-ttu-id="f7235-508">Die zugrunde liegende Standardtyp von Enumerationselementen ist **Int32.**.</span><span class="sxs-lookup"><span data-stu-id="f7235-508">The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="f7235-509">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **EnumType** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-509">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="f7235-510">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-510">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-511">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-511">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="f7235-512">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-512">Example</span></span>

<span data-ttu-id="f7235-513">Das folgende Beispiel zeigt eine **EnumType** -Element mit drei **Member** Elemente:</span><span class="sxs-lookup"><span data-stu-id="f7235-513">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="f7235-514">Function-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-514">Function Element (CSDL)</span></span>

<span data-ttu-id="f7235-515">Die **Funktion** -Element in konzeptioneller Schemadefinitionssprache (CSDL) wird verwendet, um zu definieren oder Deklarieren von Funktionen im konzeptionellen Modell.</span><span class="sxs-lookup"><span data-stu-id="f7235-515">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="f7235-516">Eine Funktion wird mit einem DefiningExpression-Element definiert.</span><span class="sxs-lookup"><span data-stu-id="f7235-516">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="f7235-517">Ein **Funktion** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="f7235-517">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f7235-518">Dokumentation (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="f7235-518">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="f7235-519">Parameter (null oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-519">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="f7235-520">DefiningExpression (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="f7235-520">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="f7235-521">ReturnType (Funktion) (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="f7235-521">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="f7235-522">Anmerkungselemente (null oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-522">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="f7235-523">Ein Rückgabewert für eine Funktion muss angegeben werden, entweder mit der **ReturnType** (Funktion) Element oder die **ReturnType** -Attribut (siehe unten), aber nicht beides.</span><span class="sxs-lookup"><span data-stu-id="f7235-523">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="f7235-524">Die möglichen Rückgabetypen sind alle EdmSimpleType-Typen, Entitätstypen, komplexe Typen, Zeilentypen (oder eine Auflistung eines dieser Typen) oder Ref-Typen.</span><span class="sxs-lookup"><span data-stu-id="f7235-524">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-525">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-525">Applicable Attributes</span></span>

<span data-ttu-id="f7235-526">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Funktion** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-526">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="f7235-527">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-527">Attribute Name</span></span> | <span data-ttu-id="f7235-528">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-528">Is Required</span></span> | <span data-ttu-id="f7235-529">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-529">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="f7235-530">**Name**</span><span class="sxs-lookup"><span data-stu-id="f7235-530">**Name**</span></span>       | <span data-ttu-id="f7235-531">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-531">Yes</span></span>         | <span data-ttu-id="f7235-532">Der Name der Funktion.</span><span class="sxs-lookup"><span data-stu-id="f7235-532">The name of the function.</span></span>          |
| <span data-ttu-id="f7235-533">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="f7235-533">**ReturnType**</span></span> | <span data-ttu-id="f7235-534">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-534">No</span></span>          | <span data-ttu-id="f7235-535">Der von der Funktion zurückgegebene Typ.</span><span class="sxs-lookup"><span data-stu-id="f7235-535">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="f7235-536">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Funktion** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-536">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="f7235-537">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-537">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-538">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-538">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="f7235-539">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-539">Example</span></span>

<span data-ttu-id="f7235-540">Im folgenden Beispiel wird eine **Funktion** Element eine Funktion definiert, die die Anzahl der Jahre zurückgibt, da die Einstellung einer Lehrkraft vergangen.</span><span class="sxs-lookup"><span data-stu-id="f7235-540">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="f7235-541">FunctionImport-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-541">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="f7235-542">Die **FunctionImport** -Element in konzeptioneller Schemadefinitionssprache (CSDL) stellt eine Funktion, die in der Datenquelle definiert, aber für andere Objekte über das konzeptionelle Modell verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-542">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="f7235-543">Beispielsweise kann im Speichermodell ein Function-Element verwendet werden, um eine gespeicherte Prozedur in einer Datenbank darzustellen.</span><span class="sxs-lookup"><span data-stu-id="f7235-543">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="f7235-544">Ein **FunctionImport** -Element im konzeptionellen Modell stellt die entsprechende Funktion in einer Entity Framework-Anwendung und der speichermodellfunktion mithilfe des FunctionImportMapping-Elements zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-544">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="f7235-545">Wird die Funktion in der Anwendung aufgerufen, wird die entsprechende gespeicherte Prozedur in der Datenbank ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="f7235-545">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="f7235-546">Die **FunctionImport** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="f7235-546">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f7235-547">Dokumentation (0 (null) oder ein Element zugelassen)</span><span class="sxs-lookup"><span data-stu-id="f7235-547">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="f7235-548">Parameter (0 (null) oder mehrere Elemente zugelassen)</span><span class="sxs-lookup"><span data-stu-id="f7235-548">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="f7235-549">Anmerkungselemente (0 (null) oder mehrere Elemente zugelassen)</span><span class="sxs-lookup"><span data-stu-id="f7235-549">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="f7235-550">ReturnType (FunctionImport) (0 (null) oder mehrere Elemente zugelassen)</span><span class="sxs-lookup"><span data-stu-id="f7235-550">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="f7235-551">Eine **Parameter** Element sollte für jeden von der Funktion akzeptierten Parameter definiert werden.</span><span class="sxs-lookup"><span data-stu-id="f7235-551">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="f7235-552">Ein Rückgabewert für eine Funktion muss angegeben werden, entweder mit der **ReturnType** (FunctionImport)-Element oder die **ReturnType** -Attribut (siehe unten), aber nicht beides.</span><span class="sxs-lookup"><span data-stu-id="f7235-552">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="f7235-553">Der Rückgabetyp-Wert muss es sich um eine Auflistung von ComplexType, EntityType oder EdmSimpleType sein.</span><span class="sxs-lookup"><span data-stu-id="f7235-553">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-554">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-554">Applicable Attributes</span></span>

<span data-ttu-id="f7235-555">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **FunctionImport** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-555">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="f7235-556">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-556">Attribute Name</span></span>   | <span data-ttu-id="f7235-557">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-557">Is Required</span></span> | <span data-ttu-id="f7235-558">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-558">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f7235-559">**Name**</span><span class="sxs-lookup"><span data-stu-id="f7235-559">**Name**</span></span>         | <span data-ttu-id="f7235-560">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-560">Yes</span></span>         | <span data-ttu-id="f7235-561">Der Name der importierten Funktion.</span><span class="sxs-lookup"><span data-stu-id="f7235-561">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="f7235-562">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="f7235-562">**ReturnType**</span></span>   | <span data-ttu-id="f7235-563">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-563">No</span></span>          | <span data-ttu-id="f7235-564">Der Typ, den die Funktion zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="f7235-564">The type that the function returns.</span></span> <span data-ttu-id="f7235-565">Verwenden Sie dieses Attribut nicht, wenn die Funktion keinen Wert zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="f7235-565">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="f7235-566">Andernfalls muss der Wert eine Auflistung von ComplexType, EntityType oder EDMSimpleType sein.</span><span class="sxs-lookup"><span data-stu-id="f7235-566">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="f7235-567">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="f7235-567">**EntitySet**</span></span>    | <span data-ttu-id="f7235-568">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-568">No</span></span>          | <span data-ttu-id="f7235-569">Wenn die Funktion eine Auflistung von Entitätstypen zurückgibt Typen verwenden, wird der Wert von der **EntitySet** muss der Entitätenmenge entsprechen, zu dem die Auflistung gehört.</span><span class="sxs-lookup"><span data-stu-id="f7235-569">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="f7235-570">Andernfalls die **EntitySet** Attribut darf nicht verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="f7235-570">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="f7235-571">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="f7235-571">**IsComposable**</span></span> | <span data-ttu-id="f7235-572">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-572">No</span></span>          | <span data-ttu-id="f7235-573">Wenn der Wert festgelegt ist, auf "true", die Funktion ist effizienter, zusammensetzbarer (Table-valued Function) und kann in einer LINQ-Abfrage verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="f7235-573">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span>  <span data-ttu-id="f7235-574">Der Standardwert ist **FALSE**.</span><span class="sxs-lookup"><span data-stu-id="f7235-574">The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="f7235-575">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **FunctionImport** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-575">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="f7235-576">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-576">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-577">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-577">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="f7235-578">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-578">Example</span></span>

<span data-ttu-id="f7235-579">Das folgende Beispiel zeigt eine **FunctionImport** Element, das einen Parameter akzeptiert und eine Auflistung von Entitätstypen zurückgibt:</span><span class="sxs-lookup"><span data-stu-id="f7235-579">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="f7235-580">Key-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-580">Key Element (CSDL)</span></span>

<span data-ttu-id="f7235-581">Die **Schlüssel** Element ist ein untergeordnetes Element des EntityType-Elements und definiert eine *Entitätsschlüssel* (eine Eigenschaft oder einen Satz von Eigenschaften eines Entitätstyps, der Identität bestimmt).</span><span class="sxs-lookup"><span data-stu-id="f7235-581">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="f7235-582">Die Eigenschaften, die einen Entitätsschlüssel bilden, werden zur Entwurfszeit ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="f7235-582">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="f7235-583">Die Werte von Entitätsschlüsseleigenschaften müssen zur Laufzeit eindeutig eine Entitätstypinstanz innerhalb eines Entitätssatzes identifizieren.</span><span class="sxs-lookup"><span data-stu-id="f7235-583">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="f7235-584">Die Eigenschaften, die einen Entitätsschlüssel bilden, sollten so ausgewählt werden, dass die Eindeutigkeit von Instanzen in einem Entitätssatz gewährleistet ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-584">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="f7235-585">Die **Schlüssel** Element definiert einen Entitätsschlüssel durch Verweisen auf eine oder mehrere der Eigenschaften eines Entitätstyps handelt.</span><span class="sxs-lookup"><span data-stu-id="f7235-585">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="f7235-586">Die **Schlüssel** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="f7235-586">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="f7235-587">PropertyRef (eine oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-587">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="f7235-588">Anmerkungselemente (null oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-588">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-589">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-589">Applicable Attributes</span></span>

<span data-ttu-id="f7235-590">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Schlüssel** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-590">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="f7235-591">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-591">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-592">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-592">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="f7235-593">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-593">Example</span></span>

<span data-ttu-id="f7235-594">Das folgende Beispiel definiert einen Entitätstyp, der mit dem Namen **Buch**.</span><span class="sxs-lookup"><span data-stu-id="f7235-594">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="f7235-595">Der Entitätsschlüssel wird definiert durch Verweisen auf die **ISBN** Eigenschaft des Entitätstyps.</span><span class="sxs-lookup"><span data-stu-id="f7235-595">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="f7235-596">Die **ISBN** Eigenschaft ist eine gute Wahl für den Entitätsschlüssel aus, da ein internationaler Standard Buch Anzahl (ISBN) ein Buch eindeutig identifiziert.</span><span class="sxs-lookup"><span data-stu-id="f7235-596">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="f7235-597">Das folgende Beispiel zeigt einen Entitätstyp (**Autor**), die einen Entitätsschlüssel, der aus zwei Eigenschaften besteht, hat **Namen** und **Adresse**.</span><span class="sxs-lookup"><span data-stu-id="f7235-597">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

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
 

<span data-ttu-id="f7235-598">Mithilfe von **Namen** und **Adresse** für die Entität ist empfehlenswert, da zwei Autoren mit demselben Namen wahrscheinlich nicht an derselben Adresse live sind.</span><span class="sxs-lookup"><span data-stu-id="f7235-598">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="f7235-599">Dieser Entitätsschlüssel garantiert jedoch nicht absolut eindeutige Entitätsschlüssel in einem Entitätssatz.</span><span class="sxs-lookup"><span data-stu-id="f7235-599">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="f7235-600">Hinzufügen einer Eigenschaft, z. B. **AutorID**, die verwendet werden kann, zur eindeutigen Identifizierung ein Autors empfiehlt sich in diesem Fall.</span><span class="sxs-lookup"><span data-stu-id="f7235-600">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="f7235-601">Member-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-601">Member Element (CSDL)</span></span>

<span data-ttu-id="f7235-602">Die **Member** Element ist ein untergeordnetes Element des Elements EnumType und ein Mitglied über den enumerierten Typ definiert.</span><span class="sxs-lookup"><span data-stu-id="f7235-602">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-603">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-603">Applicable Attributes</span></span>

<span data-ttu-id="f7235-604">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **FunctionImport** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-604">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="f7235-605">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-605">Attribute Name</span></span> | <span data-ttu-id="f7235-606">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-606">Is Required</span></span> | <span data-ttu-id="f7235-607">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-607">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f7235-608">**Name**</span><span class="sxs-lookup"><span data-stu-id="f7235-608">**Name**</span></span>       | <span data-ttu-id="f7235-609">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-609">Yes</span></span>         | <span data-ttu-id="f7235-610">Der Name des Members.</span><span class="sxs-lookup"><span data-stu-id="f7235-610">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="f7235-611">**Wert**</span><span class="sxs-lookup"><span data-stu-id="f7235-611">**Value**</span></span>      | <span data-ttu-id="f7235-612">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-612">No</span></span>          | <span data-ttu-id="f7235-613">Der Wert des Members.</span><span class="sxs-lookup"><span data-stu-id="f7235-613">The value of the member.</span></span> <span data-ttu-id="f7235-614">Wird standardmäßig das erste Element hat den Wert 0, und der Wert jedes nachfolgenden Enumerators wird um 1 erhöht.</span><span class="sxs-lookup"><span data-stu-id="f7235-614">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="f7235-615">Mehrere Elemente mit den gleichen Werten können vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="f7235-615">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="f7235-616">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **FunctionImport** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-616">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="f7235-617">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-617">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-618">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-618">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="f7235-619">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-619">Example</span></span>

<span data-ttu-id="f7235-620">Das folgende Beispiel zeigt eine **EnumType** -Element mit drei **Member** Elemente:</span><span class="sxs-lookup"><span data-stu-id="f7235-620">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="f7235-621">NavigationProperty-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-621">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="f7235-622">Ein **NavigationProperty** -Element definiert eine Navigationseigenschaft, die einen Verweis auf das andere Ende einer Zuordnung bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="f7235-622">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="f7235-623">Im Gegensatz zu Eigenschaften, die mit dem Property-Element definiert werden werden Navigationseigenschaften nicht die Form und die Eigenschaften der Daten definieren.</span><span class="sxs-lookup"><span data-stu-id="f7235-623">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="f7235-624">Sie bieten eine Möglichkeit, eine Zuordnung zwischen zwei Entitätstypen zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="f7235-624">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="f7235-625">Beachten Sie, dass Navigationseigenschaften für beide Entitätstypen an den Enden einer Zuordnung optional sind.</span><span class="sxs-lookup"><span data-stu-id="f7235-625">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="f7235-626">Wenn Sie für einen Entitätstyp am Ende einer Zuordnung eine Navigationseigenschaft definieren, muss keine Navigationseigenschaft für den Entitätstyp am anderen Ende der Zuordnung definiert werden.</span><span class="sxs-lookup"><span data-stu-id="f7235-626">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="f7235-627">Der von einer Navigationseigenschaft zurückgegebene Datentyp wird von der Multiplizität des Remotezuordnungsendes bestimmt.</span><span class="sxs-lookup"><span data-stu-id="f7235-627">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="f7235-628">Nehmen wir beispielsweise an eine Navigationseigenschaft, **OrdersNavProp**, vorhanden ist, auf eine **Kunden** Entitätstyp und navigiert eine 1: n Zuordnung zwischen **Kunden** und  **Reihenfolge**.</span><span class="sxs-lookup"><span data-stu-id="f7235-628">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="f7235-629">Da das remotezuordnungsende für die Navigationseigenschaft eine Multiplizität viele aufweist (\*), der Datentyp ist eine Sammlung (der **Reihenfolge**).</span><span class="sxs-lookup"><span data-stu-id="f7235-629">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="f7235-630">Auf ähnliche Weise, wenn eine Navigationseigenschaft, **CustomerNavProp**, vorhanden ist, auf die **Reihenfolge** Entitätstyp, dessen Datentyp wäre **Kunden** , da die Multiplizität des remoteendes ist eins (1).</span><span class="sxs-lookup"><span data-stu-id="f7235-630">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="f7235-631">Ein **NavigationProperty** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="f7235-631">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f7235-632">Dokumentation (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="f7235-632">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="f7235-633">Anmerkungselemente (null oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-633">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-634">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-634">Applicable Attributes</span></span>

<span data-ttu-id="f7235-635">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **NavigationProperty** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-635">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="f7235-636">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-636">Attribute Name</span></span>   | <span data-ttu-id="f7235-637">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-637">Is Required</span></span> | <span data-ttu-id="f7235-638">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-638">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f7235-639">**Name**</span><span class="sxs-lookup"><span data-stu-id="f7235-639">**Name**</span></span>         | <span data-ttu-id="f7235-640">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-640">Yes</span></span>         | <span data-ttu-id="f7235-641">Der Name der Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="f7235-641">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="f7235-642">**Beziehung**</span><span class="sxs-lookup"><span data-stu-id="f7235-642">**Relationship**</span></span> | <span data-ttu-id="f7235-643">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-643">Yes</span></span>         | <span data-ttu-id="f7235-644">Der Name einer Zuordnung, die sich innerhalb des Bereichs des Modells befindet.</span><span class="sxs-lookup"><span data-stu-id="f7235-644">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="f7235-645">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="f7235-645">**ToRole**</span></span>       | <span data-ttu-id="f7235-646">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-646">Yes</span></span>         | <span data-ttu-id="f7235-647">Das Ende der Zuordnung, an dem die Navigation endet.</span><span class="sxs-lookup"><span data-stu-id="f7235-647">The end of the association at which navigation ends.</span></span> <span data-ttu-id="f7235-648">Der Wert des der **ToRole** Attribut muss identisch mit dem Wert von einem der der **Rolle** Attribute, die für einen der Zuordnungsenden (definiert im AssociationEnd-Element) definiert.</span><span class="sxs-lookup"><span data-stu-id="f7235-648">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="f7235-649">**FromRole**</span><span class="sxs-lookup"><span data-stu-id="f7235-649">**FromRole**</span></span>     | <span data-ttu-id="f7235-650">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-650">Yes</span></span>         | <span data-ttu-id="f7235-651">Das Ende der Zuordnung, an dem die Navigation beginnt.</span><span class="sxs-lookup"><span data-stu-id="f7235-651">The end of the association from which navigation begins.</span></span> <span data-ttu-id="f7235-652">Der Wert des der **FromRole** Attribut muss identisch mit dem Wert von einem der der **Rolle** Attribute, die für einen der Zuordnungsenden (definiert im AssociationEnd-Element) definiert.</span><span class="sxs-lookup"><span data-stu-id="f7235-652">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="f7235-653">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **NavigationProperty** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-653">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="f7235-654">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-654">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-655">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-655">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="f7235-656">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-656">Example</span></span>

<span data-ttu-id="f7235-657">Das folgende Beispiel definiert einen Entitätstyp (**Buch**) mit zwei Navigationseigenschaften (**PublishedBy** und **WrittenBy**):</span><span class="sxs-lookup"><span data-stu-id="f7235-657">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

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
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="f7235-658">OnDelete-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-658">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="f7235-659">Die **OnDelete** -Element in konzeptioneller Schemadefinitionssprache (CSDL) definiert, Verhalten, das mit einer Zuordnung verbunden ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-659">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="f7235-660">Wenn die **Aktion** -Attributsatz auf **Cascade** an einem Ende einer Zuordnung im Zusammenhang Entitätstypen am anderen Ende der Zuordnung werden gelöscht, wenn der Entitätstyp am ersten Ende gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-660">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="f7235-661">Wenn die Zuordnung zwischen zwei Entitätstypen eine Primärschlüssel-zu-Primärschlüssel-Beziehung, wird ein geladenes abhängiges Objekt gelöscht, wenn das Prinzipalobjekt am anderen Ende der Zuordnung, unabhängig von gelöscht wird der **OnDelete** Spezifikation.</span><span class="sxs-lookup"><span data-stu-id="f7235-661">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="f7235-662">Die **OnDelete** Element wirkt sich nur auf das Laufzeitverhalten einer Anwendung, die sie nicht auf das Verhalten in der Datenquelle.</span><span class="sxs-lookup"><span data-stu-id="f7235-662">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="f7235-663">Das in der Datenquelle definierte Verhalten sollte dem in der Anwendung definierten Verhalten entsprechen.</span><span class="sxs-lookup"><span data-stu-id="f7235-663">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="f7235-664">Ein **OnDelete** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="f7235-664">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f7235-665">Dokumentation (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="f7235-665">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="f7235-666">Anmerkungselemente (null oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-666">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-667">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-667">Applicable Attributes</span></span>

<span data-ttu-id="f7235-668">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **OnDelete** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-668">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="f7235-669">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-669">Attribute Name</span></span> | <span data-ttu-id="f7235-670">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-670">Is Required</span></span> | <span data-ttu-id="f7235-671">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-671">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f7235-672">**Aktion**</span><span class="sxs-lookup"><span data-stu-id="f7235-672">**Action**</span></span>     | <span data-ttu-id="f7235-673">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-673">Yes</span></span>         | <span data-ttu-id="f7235-674">**CASCADE** oder **keine**.</span><span class="sxs-lookup"><span data-stu-id="f7235-674">**Cascade** or **None**.</span></span> <span data-ttu-id="f7235-675">Wenn **Cascade**, abhängige Entitätstypen gelöscht, sobald der prinzipalentitätstyp gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-675">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="f7235-676">Wenn **keine**, abhängige Entitätstypen nicht gelöscht, sobald der prinzipalentitätstyp gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-676">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="f7235-677">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Zuordnung** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-677">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="f7235-678">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-678">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-679">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-679">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="f7235-680">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-680">Example</span></span>

<span data-ttu-id="f7235-681">Das folgende Beispiel zeigt eine **Zuordnung** -Element, definiert der **CustomerOrders** Zuordnung.</span><span class="sxs-lookup"><span data-stu-id="f7235-681">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="f7235-682">Die **OnDelete** Element gibt an, dass alle **Bestellungen** , beziehen sich auf eine bestimmte **Kunden** und in den ObjectContext geladen wurden gelöscht werden, sobald die  **Kunden** wird gelöscht.</span><span class="sxs-lookup"><span data-stu-id="f7235-682">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="f7235-683">Parameter-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-683">Parameter Element (CSDL)</span></span>

<span data-ttu-id="f7235-684">Die **Parameter** -Element in konzeptioneller Schemadefinitionssprache (CSDL) kann ein untergeordnetes Element des FunctionImport-Elements oder Function-Element sein.</span><span class="sxs-lookup"><span data-stu-id="f7235-684">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="f7235-685">FunctionImport-Element-Anwendung</span><span class="sxs-lookup"><span data-stu-id="f7235-685">FunctionImport Element Application</span></span>

<span data-ttu-id="f7235-686">Ein **Parameter** -Element (als untergeordnetes Element der **FunctionImport** Element) wird verwendet, um die Parameter von Eingabe- und Ausgabeparameter für Funktionsimporte, die deklariert werden in CSDL definiert.</span><span class="sxs-lookup"><span data-stu-id="f7235-686">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="f7235-687">Die **Parameter** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="f7235-687">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f7235-688">Dokumentation (0 (null) oder ein Element zugelassen)</span><span class="sxs-lookup"><span data-stu-id="f7235-688">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="f7235-689">Anmerkungselemente (0 (null) oder mehrere Elemente zugelassen)</span><span class="sxs-lookup"><span data-stu-id="f7235-689">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="f7235-690">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-690">Applicable Attributes</span></span>

<span data-ttu-id="f7235-691">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Parameter** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-691">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="f7235-692">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-692">Attribute Name</span></span> | <span data-ttu-id="f7235-693">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-693">Is Required</span></span> | <span data-ttu-id="f7235-694">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-694">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f7235-695">**Name**</span><span class="sxs-lookup"><span data-stu-id="f7235-695">**Name**</span></span>       | <span data-ttu-id="f7235-696">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-696">Yes</span></span>         | <span data-ttu-id="f7235-697">Der Name des Parameters.</span><span class="sxs-lookup"><span data-stu-id="f7235-697">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="f7235-698">**Type**</span><span class="sxs-lookup"><span data-stu-id="f7235-698">**Type**</span></span>       | <span data-ttu-id="f7235-699">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-699">Yes</span></span>         | <span data-ttu-id="f7235-700">Der Typ des Parameters.</span><span class="sxs-lookup"><span data-stu-id="f7235-700">The parameter type.</span></span> <span data-ttu-id="f7235-701">Der Wert muss eine **EDMSimpleType** oder ein komplexer Typ, der innerhalb des Bereichs des Modells ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-701">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="f7235-702">**Modus**</span><span class="sxs-lookup"><span data-stu-id="f7235-702">**Mode**</span></span>       | <span data-ttu-id="f7235-703">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-703">No</span></span>          | <span data-ttu-id="f7235-704">**In**, **Out**, oder **InOut** abhängig davon, ob der Parameter eine Eingabe, Ausgabe oder Eingabe-/Ausgabeparameter.</span><span class="sxs-lookup"><span data-stu-id="f7235-704">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="f7235-705">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="f7235-705">**MaxLength**</span></span>  | <span data-ttu-id="f7235-706">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-706">No</span></span>          | <span data-ttu-id="f7235-707">Die maximal zulässige Länge des Parameters.</span><span class="sxs-lookup"><span data-stu-id="f7235-707">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="f7235-708">**Genauigkeit**</span><span class="sxs-lookup"><span data-stu-id="f7235-708">**Precision**</span></span>  | <span data-ttu-id="f7235-709">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-709">No</span></span>          | <span data-ttu-id="f7235-710">Die Genauigkeit des Parameters.</span><span class="sxs-lookup"><span data-stu-id="f7235-710">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="f7235-711">**Scale** (Skalieren)</span><span class="sxs-lookup"><span data-stu-id="f7235-711">**Scale**</span></span>      | <span data-ttu-id="f7235-712">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-712">No</span></span>          | <span data-ttu-id="f7235-713">Die Skalierung des Parameters.</span><span class="sxs-lookup"><span data-stu-id="f7235-713">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="f7235-714">**SRID**</span><span class="sxs-lookup"><span data-stu-id="f7235-714">**SRID**</span></span>       | <span data-ttu-id="f7235-715">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-715">No</span></span>          | <span data-ttu-id="f7235-716">Räumliche Verweis Systembezeichner.</span><span class="sxs-lookup"><span data-stu-id="f7235-716">Spatial System Reference Identifier.</span></span> <span data-ttu-id="f7235-717">Gilt nur für Parameter des räumlichen Typen.</span><span class="sxs-lookup"><span data-stu-id="f7235-717">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="f7235-718">Weitere Informationen finden Sie unter [SRID](http://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="f7235-718">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="f7235-719">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Parameter** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-719">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="f7235-720">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-720">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-721">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-721">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="f7235-722">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-722">Example</span></span>

<span data-ttu-id="f7235-723">Das folgende Beispiel zeigt eine **FunctionImport** -Element mit einem **Parameter** untergeordnetes Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-723">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="f7235-724">Die Funktion akzeptiert einen Eingabeparameter und gibt eine Auflistung von Entitätstypen zurück.</span><span class="sxs-lookup"><span data-stu-id="f7235-724">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="f7235-725">Function-Element-Anwendung</span><span class="sxs-lookup"><span data-stu-id="f7235-725">Function Element Application</span></span>

<span data-ttu-id="f7235-726">Ein **Parameter** -Element (als untergeordnetes Element der **Funktion** Element) definiert Parameter für Funktionen, die definiert oder deklariert werden in einem konzeptionellen Modell.</span><span class="sxs-lookup"><span data-stu-id="f7235-726">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="f7235-727">Die **Parameter** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="f7235-727">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f7235-728">Dokumentation (0 oder 1-Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-728">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="f7235-729">CollectionType (0 oder 1-Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-729">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="f7235-730">ReferenceType (0 oder 1-Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-730">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="f7235-731">RowType (0 oder 1-Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-731">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="f7235-732">Nur eines der der **CollectionType**, **ReferenceType**, oder **RowType** Elemente können ein untergeordnetes Element des werden eine **Eigenschaft** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-732">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="f7235-733">Anmerkungselemente (0 (null) oder mehrere Elemente zugelassen)</span><span class="sxs-lookup"><span data-stu-id="f7235-733">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="f7235-734">Anmerkungselemente müssen an alle anderen untergeordneten Elemente angereiht werden.</span><span class="sxs-lookup"><span data-stu-id="f7235-734">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="f7235-735">Anmerkungselemente sind nur zulässig, in der CSDL-v2 und höher.</span><span class="sxs-lookup"><span data-stu-id="f7235-735">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="f7235-736">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-736">Applicable Attributes</span></span>

<span data-ttu-id="f7235-737">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Parameter** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-737">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="f7235-738">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-738">Attribute Name</span></span>   | <span data-ttu-id="f7235-739">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-739">Is Required</span></span> | <span data-ttu-id="f7235-740">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-740">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f7235-741">**Name**</span><span class="sxs-lookup"><span data-stu-id="f7235-741">**Name**</span></span>         | <span data-ttu-id="f7235-742">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-742">Yes</span></span>         | <span data-ttu-id="f7235-743">Der Name des Parameters.</span><span class="sxs-lookup"><span data-stu-id="f7235-743">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="f7235-744">**Type**</span><span class="sxs-lookup"><span data-stu-id="f7235-744">**Type**</span></span>         | <span data-ttu-id="f7235-745">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-745">No</span></span>          | <span data-ttu-id="f7235-746">Der Typ des Parameters.</span><span class="sxs-lookup"><span data-stu-id="f7235-746">The parameter type.</span></span> <span data-ttu-id="f7235-747">Ein Parameter kann einer der folgenden Typen (oder Auflistungen dieser Typen) sein:</span><span class="sxs-lookup"><span data-stu-id="f7235-747">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="f7235-748">**EdmSimpleType**</span><span class="sxs-lookup"><span data-stu-id="f7235-748">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="f7235-749">Entitätstyp</span><span class="sxs-lookup"><span data-stu-id="f7235-749">entity type</span></span> <br/> <span data-ttu-id="f7235-750">Komplexer Typ</span><span class="sxs-lookup"><span data-stu-id="f7235-750">complex type</span></span> <br/> <span data-ttu-id="f7235-751">Zeilentyp</span><span class="sxs-lookup"><span data-stu-id="f7235-751">row type</span></span> <br/> <span data-ttu-id="f7235-752">Verweistyp</span><span class="sxs-lookup"><span data-stu-id="f7235-752">reference type</span></span>                             |
| <span data-ttu-id="f7235-753">**NULL-Werte zulässt**</span><span class="sxs-lookup"><span data-stu-id="f7235-753">**Nullable**</span></span>     | <span data-ttu-id="f7235-754">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-754">No</span></span>          | <span data-ttu-id="f7235-755">**"True"** (Standardwert) oder **"false"** abhängig davon, ob die Eigenschaft kann einen **null** Wert.</span><span class="sxs-lookup"><span data-stu-id="f7235-755">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="f7235-756">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="f7235-756">**DefaultValue**</span></span> | <span data-ttu-id="f7235-757">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-757">No</span></span>          | <span data-ttu-id="f7235-758">Der Standardwert der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="f7235-758">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="f7235-759">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="f7235-759">**MaxLength**</span></span>    | <span data-ttu-id="f7235-760">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-760">No</span></span>          | <span data-ttu-id="f7235-761">Die maximale Länge des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="f7235-761">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="f7235-762">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="f7235-762">**FixedLength**</span></span>  | <span data-ttu-id="f7235-763">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-763">No</span></span>          | <span data-ttu-id="f7235-764">**"True"** oder **"false"** abhängig davon, ob der Eigenschaftswert als Zeichenfolge mit fester Länge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-764">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="f7235-765">**Genauigkeit**</span><span class="sxs-lookup"><span data-stu-id="f7235-765">**Precision**</span></span>    | <span data-ttu-id="f7235-766">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-766">No</span></span>          | <span data-ttu-id="f7235-767">Die Genauigkeit des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="f7235-767">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="f7235-768">**Scale** (Skalieren)</span><span class="sxs-lookup"><span data-stu-id="f7235-768">**Scale**</span></span>        | <span data-ttu-id="f7235-769">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-769">No</span></span>          | <span data-ttu-id="f7235-770">Die Skalierung des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="f7235-770">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="f7235-771">**SRID**</span><span class="sxs-lookup"><span data-stu-id="f7235-771">**SRID**</span></span>         | <span data-ttu-id="f7235-772">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-772">No</span></span>          | <span data-ttu-id="f7235-773">Räumliche Verweis Systembezeichner.</span><span class="sxs-lookup"><span data-stu-id="f7235-773">Spatial System Reference Identifier.</span></span> <span data-ttu-id="f7235-774">Gilt nur für Eigenschaften des räumlichen Typen.</span><span class="sxs-lookup"><span data-stu-id="f7235-774">Valid only for properties of spatial types.</span></span> <span data-ttu-id="f7235-775">Weitere Informationen finden Sie unter [SRID](http://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="f7235-775">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="f7235-776">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="f7235-776">**Unicode**</span></span>      | <span data-ttu-id="f7235-777">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-777">No</span></span>          | <span data-ttu-id="f7235-778">**"True"** oder **"false"** abhängig davon, ob der Eigenschaftswert als Unicode-Zeichenfolge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-778">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="f7235-779">**Sortierung**</span><span class="sxs-lookup"><span data-stu-id="f7235-779">**Collation**</span></span>    | <span data-ttu-id="f7235-780">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-780">No</span></span>          | <span data-ttu-id="f7235-781">Eine Zeichenfolge, die die Sortierreihenfolge angibt, die in der Datenquelle verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="f7235-781">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="f7235-782">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Parameter** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-782">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="f7235-783">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-783">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-784">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-784">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="f7235-785">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-785">Example</span></span>

<span data-ttu-id="f7235-786">Das folgende Beispiel zeigt eine **Funktion** -Element, das eine verwendet **Parameter** untergeordneten-Element, um einen Funktionsparameter zu definieren.</span><span class="sxs-lookup"><span data-stu-id="f7235-786">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
``` 

 

## <a name="principal-element-csdl"></a><span data-ttu-id="f7235-787">Principal-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-787">Principal Element (CSDL)</span></span>

<span data-ttu-id="f7235-788">Die **Principal** -Element in konzeptioneller Schemadefinitionssprache (CSDL) ist ein untergeordnetes Element mit dem ReferentialConstraint-Element, das prinzipalende einer referenziellen Einschränkung definiert.</span><span class="sxs-lookup"><span data-stu-id="f7235-788">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="f7235-789">Ein **ReferentialConstraint** -Element definiert Funktionen, die eine Einschränkung der referenziellen Integrität in einer relationalen Datenbank ähnlich ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-789">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="f7235-790">So wie eine Spalte (oder Spalten) einer Datenbanktabelle auf den Primärschlüssel einer anderen Tabelle verweisen kann, kann eine Eigenschaft (oder Eigenschaften) eines Entitätstyps auf den Entitätsschlüssel eines anderen Entitätstyps verweisen.</span><span class="sxs-lookup"><span data-stu-id="f7235-790">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="f7235-791">Wird aufgerufen, der Entitätstyp, auf die verwiesen wird, wird die *prinzipalende* der Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="f7235-791">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="f7235-792">Der Entitätstyp, der auf das prinzipalende verweist heißt die *abhängigen Endes* der Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="f7235-792">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="f7235-793">**PropertyRef** Elemente werden verwendet, um anzugeben, welche der Tasten durch das abhängige Ende verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="f7235-793">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="f7235-794">Die **Principal** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="f7235-794">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f7235-795">PropertyRef (eine oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-795">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="f7235-796">Anmerkungselemente (null oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-796">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-797">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-797">Applicable Attributes</span></span>

<span data-ttu-id="f7235-798">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Principal** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-798">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="f7235-799">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-799">Attribute Name</span></span> | <span data-ttu-id="f7235-800">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-800">Is Required</span></span> | <span data-ttu-id="f7235-801">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-801">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="f7235-802">**Rolle**</span><span class="sxs-lookup"><span data-stu-id="f7235-802">**Role**</span></span>       | <span data-ttu-id="f7235-803">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-803">Yes</span></span>         | <span data-ttu-id="f7235-804">Der Name des Entitätstyps am Prinzipalende der Zuordnung.</span><span class="sxs-lookup"><span data-stu-id="f7235-804">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="f7235-805">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Principal** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-805">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="f7235-806">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-806">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-807">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-807">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="f7235-808">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-808">Example</span></span>

<span data-ttu-id="f7235-809">Das folgende Beispiel zeigt eine **ReferentialConstraint** -Element, das Teil der Definition ist die **PublishedBy** Zuordnung.</span><span class="sxs-lookup"><span data-stu-id="f7235-809">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="f7235-810">Die **Id** Eigenschaft der **Verleger** -Entitätstyps bildet das prinzipalende der referenziellen Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="f7235-810">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

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
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="f7235-811">Property-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-811">Property Element (CSDL)</span></span>

<span data-ttu-id="f7235-812">Die **Eigenschaft** -Element in konzeptioneller Schemadefinitionssprache (CSDL) kann ein untergeordnetes Element des EntityType-Element, das ComplexType-Element oder das RowType-Element sein.</span><span class="sxs-lookup"><span data-stu-id="f7235-812">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="f7235-813">EntityType- und ComplexType-Element-Anwendungen</span><span class="sxs-lookup"><span data-stu-id="f7235-813">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="f7235-814">**Eigenschaft** Elemente (als untergeordnete Elemente des **EntityType** oder **ComplexType** Elemente) definieren die Form und die Merkmale der Daten, die eine Entitätstypinstanz oder komplexe Typinstanz enthält .</span><span class="sxs-lookup"><span data-stu-id="f7235-814">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="f7235-815">Eigenschaften in einem konzeptionellen Modell sind analog zu den Eigenschaften, die für eine Klasse definiert werden.</span><span class="sxs-lookup"><span data-stu-id="f7235-815">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="f7235-816">So wie Eigenschaften die Form einer Klasse definieren und Informationen zu Objekten enthalten definieren Eigenschaften in einem konzeptionellen Modell die Form eines Entitätstyps und enthalten Informationen zu Entitätstypinstanzen.</span><span class="sxs-lookup"><span data-stu-id="f7235-816">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="f7235-817">Die **Eigenschaft** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="f7235-817">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f7235-818">Documentation-Element (null oder ein Element zugelassen)</span><span class="sxs-lookup"><span data-stu-id="f7235-818">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="f7235-819">Anmerkungselemente (0 (null) oder mehrere Elemente zugelassen)</span><span class="sxs-lookup"><span data-stu-id="f7235-819">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="f7235-820">Die folgenden Facets können angewendet werden, um eine **Eigenschaft** Element: **Nullable**, **DefaultValue**, **MaxLength**,  **FixedLength**, **Genauigkeit**, **Skalierung**, **Unicode**, **Sortierreihenfolge**,  **ConcurrencyMode**.</span><span class="sxs-lookup"><span data-stu-id="f7235-820">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="f7235-821">Facets sind XML-Attribute, die Informationen über die Speicherung von Eigenschaftswerten im Datenspeicher bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="f7235-821">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="f7235-822">Facets können nur auf Eigenschaften des Typs angewendet werden **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="f7235-822">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="f7235-823">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-823">Applicable Attributes</span></span>

<span data-ttu-id="f7235-824">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Eigenschaft** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-824">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="f7235-825">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-825">Attribute Name</span></span>                                                         | <span data-ttu-id="f7235-826">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-826">Is Required</span></span> | <span data-ttu-id="f7235-827">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-827">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f7235-828">**Name**</span><span class="sxs-lookup"><span data-stu-id="f7235-828">**Name**</span></span>                                                               | <span data-ttu-id="f7235-829">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-829">Yes</span></span>         | <span data-ttu-id="f7235-830">Den Namen der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="f7235-830">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="f7235-831">**Type**</span><span class="sxs-lookup"><span data-stu-id="f7235-831">**Type**</span></span>                                                               | <span data-ttu-id="f7235-832">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-832">Yes</span></span>         | <span data-ttu-id="f7235-833">Der Typ des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="f7235-833">The type of the property value.</span></span> <span data-ttu-id="f7235-834">Der Eigenschaftswerttyp muss ein **EDMSimpleType** oder ein komplexer Typ (durch einen vollqualifizierten Namen angegeben), der im Gültigkeitsbereich des Modells liegt.</span><span class="sxs-lookup"><span data-stu-id="f7235-834">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="f7235-835">**NULL-Werte zulässt**</span><span class="sxs-lookup"><span data-stu-id="f7235-835">**Nullable**</span></span>                                                           | <span data-ttu-id="f7235-836">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-836">No</span></span>          | <span data-ttu-id="f7235-837">**"True"** (Standardwert) oder <strong>"false"</strong> abhängig davon, ob die Eigenschaft einen null-Wert haben kann.</span><span class="sxs-lookup"><span data-stu-id="f7235-837">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="f7235-838">> In der CSDL-Version 1 Eigenschaft eines komplexen Typs benötigen `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="f7235-838">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="f7235-839">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="f7235-839">**DefaultValue**</span></span>                                                       | <span data-ttu-id="f7235-840">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-840">No</span></span>          | <span data-ttu-id="f7235-841">Der Standardwert der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="f7235-841">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="f7235-842">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="f7235-842">**MaxLength**</span></span>                                                          | <span data-ttu-id="f7235-843">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-843">No</span></span>          | <span data-ttu-id="f7235-844">Die maximale Länge des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="f7235-844">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="f7235-845">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="f7235-845">**FixedLength**</span></span>                                                        | <span data-ttu-id="f7235-846">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-846">No</span></span>          | <span data-ttu-id="f7235-847">**"True"** oder **"false"** abhängig davon, ob der Eigenschaftswert als Zeichenfolge mit fester Länge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-847">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="f7235-848">**Genauigkeit**</span><span class="sxs-lookup"><span data-stu-id="f7235-848">**Precision**</span></span>                                                          | <span data-ttu-id="f7235-849">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-849">No</span></span>          | <span data-ttu-id="f7235-850">Die Genauigkeit des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="f7235-850">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="f7235-851">**Scale** (Skalieren)</span><span class="sxs-lookup"><span data-stu-id="f7235-851">**Scale**</span></span>                                                              | <span data-ttu-id="f7235-852">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-852">No</span></span>          | <span data-ttu-id="f7235-853">Die Skalierung des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="f7235-853">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="f7235-854">**SRID**</span><span class="sxs-lookup"><span data-stu-id="f7235-854">**SRID**</span></span>                                                               | <span data-ttu-id="f7235-855">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-855">No</span></span>          | <span data-ttu-id="f7235-856">Räumliche Verweis Systembezeichner.</span><span class="sxs-lookup"><span data-stu-id="f7235-856">Spatial System Reference Identifier.</span></span> <span data-ttu-id="f7235-857">Gilt nur für Eigenschaften des räumlichen Typen.</span><span class="sxs-lookup"><span data-stu-id="f7235-857">Valid only for properties of spatial types.</span></span> <span data-ttu-id="f7235-858">Weitere Informationen finden Sie unter [SRID](http://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="f7235-858">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="f7235-859">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="f7235-859">**Unicode**</span></span>                                                            | <span data-ttu-id="f7235-860">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-860">No</span></span>          | <span data-ttu-id="f7235-861">**"True"** oder **"false"** abhängig davon, ob der Eigenschaftswert als Unicode-Zeichenfolge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-861">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="f7235-862">**Sortierung**</span><span class="sxs-lookup"><span data-stu-id="f7235-862">**Collation**</span></span>                                                          | <span data-ttu-id="f7235-863">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-863">No</span></span>          | <span data-ttu-id="f7235-864">Eine Zeichenfolge, die die Sortierreihenfolge angibt, die in der Datenquelle verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="f7235-864">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="f7235-865">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="f7235-865">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="f7235-866">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-866">No</span></span>          | <span data-ttu-id="f7235-867">**Keine** (Standardwert) oder **Fixed**.</span><span class="sxs-lookup"><span data-stu-id="f7235-867">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="f7235-868">Wenn der Wert, um festgelegt ist **Fixed**, den Wert der Eigenschaft an Überprüfungen auf vollständige Parallelität verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="f7235-868">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="f7235-869">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Eigenschaft** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-869">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="f7235-870">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-870">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-871">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-871">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="f7235-872">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-872">Example</span></span>

<span data-ttu-id="f7235-873">Das folgende Beispiel zeigt eine **EntityType** -Element mit drei **Eigenschaft** Elemente:</span><span class="sxs-lookup"><span data-stu-id="f7235-873">The following example shows an **EntityType** element with three **Property** elements:</span></span>

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
 

<span data-ttu-id="f7235-874">Das folgende Beispiel zeigt eine **ComplexType** -Element mit fünf **Eigenschaft** Elemente:</span><span class="sxs-lookup"><span data-stu-id="f7235-874">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="f7235-875">RowType-Element-Anwendung</span><span class="sxs-lookup"><span data-stu-id="f7235-875">RowType Element Application</span></span>

<span data-ttu-id="f7235-876">**Eigenschaft** Elemente (als untergeordnete Elemente von einem **RowType** Elements) definieren die Form und die Eigenschaften der Daten, die, die an oder von einer modelldefinierten Funktion zurückgegeben werden können.</span><span class="sxs-lookup"><span data-stu-id="f7235-876">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="f7235-877">Die **Eigenschaft** Element kann nur jeweils eines der folgenden untergeordneten Elemente verfügen:</span><span class="sxs-lookup"><span data-stu-id="f7235-877">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="f7235-878">CollectionType</span><span class="sxs-lookup"><span data-stu-id="f7235-878">CollectionType</span></span>
-   <span data-ttu-id="f7235-879">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="f7235-879">ReferenceType</span></span>
-   <span data-ttu-id="f7235-880">RowType</span><span class="sxs-lookup"><span data-stu-id="f7235-880">RowType</span></span>

<span data-ttu-id="f7235-881">Die **Eigenschaft** -Element kann eine beliebige Anzahl Anmerkung Unterelemente aufweisen.</span><span class="sxs-lookup"><span data-stu-id="f7235-881">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="f7235-882">Anmerkungselemente sind nur zulässig, in der CSDL-v2 und höher.</span><span class="sxs-lookup"><span data-stu-id="f7235-882">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="f7235-883">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-883">Applicable Attributes</span></span>

<span data-ttu-id="f7235-884">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Eigenschaft** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-884">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="f7235-885">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-885">Attribute Name</span></span>                                                     | <span data-ttu-id="f7235-886">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-886">Is Required</span></span> | <span data-ttu-id="f7235-887">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-887">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f7235-888">**Name**</span><span class="sxs-lookup"><span data-stu-id="f7235-888">**Name**</span></span>                                                           | <span data-ttu-id="f7235-889">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-889">Yes</span></span>         | <span data-ttu-id="f7235-890">Den Namen der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="f7235-890">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="f7235-891">**Type**</span><span class="sxs-lookup"><span data-stu-id="f7235-891">**Type**</span></span>                                                           | <span data-ttu-id="f7235-892">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-892">Yes</span></span>         | <span data-ttu-id="f7235-893">Der Typ des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="f7235-893">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="f7235-894">**NULL-Werte zulässt**</span><span class="sxs-lookup"><span data-stu-id="f7235-894">**Nullable**</span></span>                                                       | <span data-ttu-id="f7235-895">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-895">No</span></span>          | <span data-ttu-id="f7235-896">**"True"** (Standardwert) oder **"false"** abhängig davon, ob die Eigenschaft einen null-Wert haben kann.</span><span class="sxs-lookup"><span data-stu-id="f7235-896">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="f7235-897">> In CSDL v1 Eigenschaft eines komplexen Typs benötigen `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="f7235-897">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="f7235-898">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="f7235-898">**DefaultValue**</span></span>                                                   | <span data-ttu-id="f7235-899">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-899">No</span></span>          | <span data-ttu-id="f7235-900">Der Standardwert der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="f7235-900">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="f7235-901">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="f7235-901">**MaxLength**</span></span>                                                      | <span data-ttu-id="f7235-902">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-902">No</span></span>          | <span data-ttu-id="f7235-903">Die maximale Länge des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="f7235-903">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="f7235-904">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="f7235-904">**FixedLength**</span></span>                                                    | <span data-ttu-id="f7235-905">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-905">No</span></span>          | <span data-ttu-id="f7235-906">**"True"** oder **"false"** abhängig davon, ob der Eigenschaftswert als Zeichenfolge mit fester Länge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-906">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="f7235-907">**Genauigkeit**</span><span class="sxs-lookup"><span data-stu-id="f7235-907">**Precision**</span></span>                                                      | <span data-ttu-id="f7235-908">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-908">No</span></span>          | <span data-ttu-id="f7235-909">Die Genauigkeit des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="f7235-909">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="f7235-910">**Scale** (Skalieren)</span><span class="sxs-lookup"><span data-stu-id="f7235-910">**Scale**</span></span>                                                          | <span data-ttu-id="f7235-911">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-911">No</span></span>          | <span data-ttu-id="f7235-912">Die Skalierung des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="f7235-912">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="f7235-913">**SRID**</span><span class="sxs-lookup"><span data-stu-id="f7235-913">**SRID**</span></span>                                                           | <span data-ttu-id="f7235-914">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-914">No</span></span>          | <span data-ttu-id="f7235-915">Räumliche Verweis Systembezeichner.</span><span class="sxs-lookup"><span data-stu-id="f7235-915">Spatial System Reference Identifier.</span></span> <span data-ttu-id="f7235-916">Gilt nur für Eigenschaften des räumlichen Typen.</span><span class="sxs-lookup"><span data-stu-id="f7235-916">Valid only for properties of spatial types.</span></span> <span data-ttu-id="f7235-917">Weitere Informationen finden Sie unter [SRID](http://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="f7235-917">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="f7235-918">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="f7235-918">**Unicode**</span></span>                                                        | <span data-ttu-id="f7235-919">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-919">No</span></span>          | <span data-ttu-id="f7235-920">**"True"** oder **"false"** abhängig davon, ob der Eigenschaftswert als Unicode-Zeichenfolge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-920">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="f7235-921">**Sortierung**</span><span class="sxs-lookup"><span data-stu-id="f7235-921">**Collation**</span></span>                                                      | <span data-ttu-id="f7235-922">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-922">No</span></span>          | <span data-ttu-id="f7235-923">Eine Zeichenfolge, die die Sortierreihenfolge angibt, die in der Datenquelle verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="f7235-923">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="f7235-924">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Eigenschaft** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-924">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="f7235-925">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-925">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-926">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-926">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="f7235-927">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-927">Example</span></span>

<span data-ttu-id="f7235-928">Das folgende Beispiel zeigt **Eigenschaft** Elemente verwendet, um die Form des Rückgabetyps einer modelldefinierten Funktion zu definieren.</span><span class="sxs-lookup"><span data-stu-id="f7235-928">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

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
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="f7235-929">PropertyRef-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-929">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="f7235-930">Die **PropertyRef** -Element in konzeptioneller Schemadefinitionssprache (CSDL) verweist auf eine Eigenschaft eines Entitätstyps, um anzugeben, dass die Eigenschaft eine der folgenden Rollen ausführt:</span><span class="sxs-lookup"><span data-stu-id="f7235-930">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="f7235-931">Ein Teil des Entitätsschlüssels (eine Eigenschaft oder ein Satz von Eigenschaften eines Entitätstyps, der die Identität bestimmt).</span><span class="sxs-lookup"><span data-stu-id="f7235-931">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="f7235-932">Eine oder mehrere **PropertyRef** -Elemente können verwendet werden, um einen Entitätsschlüssel zu definieren.</span><span class="sxs-lookup"><span data-stu-id="f7235-932">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="f7235-933">Das abhängige Ende oder Prinzipalende einer referenziellen Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="f7235-933">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="f7235-934">Die **PropertyRef** Element kann nur Anmerkungselemente (null oder mehr) als untergeordnete Elemente verfügen.</span><span class="sxs-lookup"><span data-stu-id="f7235-934">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="f7235-935">Anmerkungselemente sind nur zulässig, in der CSDL-v2 und höher.</span><span class="sxs-lookup"><span data-stu-id="f7235-935">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-936">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-936">Applicable Attributes</span></span>

<span data-ttu-id="f7235-937">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **PropertyRef** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-937">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="f7235-938">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-938">Attribute Name</span></span> | <span data-ttu-id="f7235-939">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-939">Is Required</span></span> | <span data-ttu-id="f7235-940">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-940">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="f7235-941">**Name**</span><span class="sxs-lookup"><span data-stu-id="f7235-941">**Name**</span></span>       | <span data-ttu-id="f7235-942">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-942">Yes</span></span>         | <span data-ttu-id="f7235-943">Der Name der referenzierten Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="f7235-943">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="f7235-944">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **PropertyRef** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-944">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="f7235-945">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-945">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-946">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-946">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="f7235-947">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-947">Example</span></span>

<span data-ttu-id="f7235-948">Das folgende Beispiel definiert einen Entitätstyp (**Buch**).</span><span class="sxs-lookup"><span data-stu-id="f7235-948">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="f7235-949">Der Entitätsschlüssel wird definiert durch Verweisen auf die **ISBN** Eigenschaft des Entitätstyps.</span><span class="sxs-lookup"><span data-stu-id="f7235-949">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

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
 

<span data-ttu-id="f7235-950">Im nächsten Beispiel zwei **PropertyRef** Elemente werden verwendet, um anzugeben, dass zwei Eigenschaften (**Id** und **PublisherId**) sind das prinzipalende und das abhängige Ende einer referenziellen Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="f7235-950">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

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
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="f7235-951">ReferenceType-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-951">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="f7235-952">Die **ReferenceType** -Element in konzeptioneller Schemadefinitionssprache (CSDL) gibt einen Verweis auf einen Entitätstyp an.</span><span class="sxs-lookup"><span data-stu-id="f7235-952">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="f7235-953">Die **ReferenceType** Element kann ein untergeordnetes Element der folgenden Elemente sein:</span><span class="sxs-lookup"><span data-stu-id="f7235-953">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="f7235-954">ReturnType (Funktion)</span><span class="sxs-lookup"><span data-stu-id="f7235-954">ReturnType (Function)</span></span>
-   <span data-ttu-id="f7235-955">Parameter</span><span class="sxs-lookup"><span data-stu-id="f7235-955">Parameter</span></span>
-   <span data-ttu-id="f7235-956">CollectionType</span><span class="sxs-lookup"><span data-stu-id="f7235-956">CollectionType</span></span>

<span data-ttu-id="f7235-957">Die **ReferenceType** Element wird verwendet, wenn ein Parameter oder Rückgabetyp für eine Funktion zu definieren.</span><span class="sxs-lookup"><span data-stu-id="f7235-957">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="f7235-958">Ein **ReferenceType** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="f7235-958">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f7235-959">Dokumentation (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="f7235-959">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="f7235-960">Anmerkungselemente (null oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-960">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-961">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-961">Applicable Attributes</span></span>

<span data-ttu-id="f7235-962">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **ReferenceType** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-962">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="f7235-963">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-963">Attribute Name</span></span> | <span data-ttu-id="f7235-964">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-964">Is Required</span></span> | <span data-ttu-id="f7235-965">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-965">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="f7235-966">**Type**</span><span class="sxs-lookup"><span data-stu-id="f7235-966">**Type**</span></span>       | <span data-ttu-id="f7235-967">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-967">Yes</span></span>         | <span data-ttu-id="f7235-968">Der Name des Entitätstyps, auf den verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-968">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="f7235-969">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **ReferenceType** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-969">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="f7235-970">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-970">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-971">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-971">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="f7235-972">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-972">Example</span></span>

<span data-ttu-id="f7235-973">Das folgende Beispiel zeigt die **ReferenceType** -Element als untergeordnetes Element des eine **Parameter** Element in einer modelldefinierten Funktion, die akzeptiert einen Verweis auf eine **Person** Entität Typ:</span><span class="sxs-lookup"><span data-stu-id="f7235-973">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

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
 

<span data-ttu-id="f7235-974">Das folgende Beispiel zeigt die **ReferenceType** -Element als untergeordnetes Element des eine **ReturnType** (Funktion)-Element in einer modelldefinierten Funktion, die einen Verweis auf eine **Person**Entitätstyp:</span><span class="sxs-lookup"><span data-stu-id="f7235-974">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

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
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="f7235-975">ReferentialConstraint-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-975">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="f7235-976">Ein **ReferentialConstraint** -Element in konzeptioneller Schemadefinitionssprache (CSDL) definiert die Funktionalität, die eine Einschränkung der referenziellen Integrität in einer relationalen Datenbank ähnlich ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-976">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="f7235-977">So wie eine Spalte (oder Spalten) einer Datenbanktabelle auf den Primärschlüssel einer anderen Tabelle verweisen kann, kann eine Eigenschaft (oder Eigenschaften) eines Entitätstyps auf den Entitätsschlüssel eines anderen Entitätstyps verweisen.</span><span class="sxs-lookup"><span data-stu-id="f7235-977">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="f7235-978">Wird aufgerufen, der Entitätstyp, auf die verwiesen wird, wird die *prinzipalende* der Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="f7235-978">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="f7235-979">Der Entitätstyp, der auf das prinzipalende verweist heißt die *abhängigen Endes* der Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="f7235-979">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="f7235-980">Wenn ein Fremdschlüssel, die für einen Entitätstyp verfügbar gemacht ist eine Eigenschaft eines anderen Entitätstyps verweist die **ReferentialConstraint** -Element definiert eine Zuordnung zwischen den zwei Entitätstypen.</span><span class="sxs-lookup"><span data-stu-id="f7235-980">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="f7235-981">Da die **ReferentialConstraint** Element enthält Informationen wie zwei Entitätstypen verwandt sind, wird keine entsprechende AssociationSetMapping-Element ist in der Mapping-Spezifikationssprache (MSL) erforderlich.</span><span class="sxs-lookup"><span data-stu-id="f7235-981">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="f7235-982">Eine Zuordnung zwischen zwei Entitätstypen, denen keine Fremdschlüssel verfügbar gemacht werden müssen, eine entsprechende **AssociationSetMapping** Element, um die Datenquelle Zuordnungsinformationen zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="f7235-982">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="f7235-983">Wenn ein Fremdschlüssel nicht für einen Entitätstyp verfügbar gemacht wird die **ReferentialConstraint** -Element kann nur eine primary Key-zu-Primärschlüssel-Einschränkung zwischen dem Entitätstyp und einem anderen Entitätstyp definieren.</span><span class="sxs-lookup"><span data-stu-id="f7235-983">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="f7235-984">Ein **ReferentialConstraint** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="f7235-984">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f7235-985">Dokumentation (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="f7235-985">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="f7235-986">Prinzipal (genau ein Element)</span><span class="sxs-lookup"><span data-stu-id="f7235-986">Principal (exactly one element)</span></span>
-   <span data-ttu-id="f7235-987">Abhängige (genau ein Element)</span><span class="sxs-lookup"><span data-stu-id="f7235-987">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="f7235-988">Anmerkungselemente (null oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-988">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-989">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-989">Applicable Attributes</span></span>

<span data-ttu-id="f7235-990">Die **ReferentialConstraint** -Element kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) aufweisen.</span><span class="sxs-lookup"><span data-stu-id="f7235-990">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="f7235-991">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-991">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-992">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-992">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="f7235-993">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-993">Example</span></span>

<span data-ttu-id="f7235-994">Das folgende Beispiel zeigt eine **ReferentialConstraint** Element verwendet wird, als Teil der Definition der **PublishedBy** Zuordnung.</span><span class="sxs-lookup"><span data-stu-id="f7235-994">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

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
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="f7235-995">ReturnType (Funktion)-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-995">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="f7235-996">Die **ReturnType** (Funktion)-Element in konzeptioneller Schemadefinitionssprache (CSDL) gibt an, den Rückgabetyp für eine Funktion, die in ein Function-Element definiert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-996">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="f7235-997">Eine Funktion, der Rückgabetyp kann auch angegeben werden, mit einem **ReturnType** Attribut.</span><span class="sxs-lookup"><span data-stu-id="f7235-997">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="f7235-998">Rückgabetypen können alle **EdmSimpleType**, Entitätstyp, komplexen Typ, Zeilentyp, Ref-Typ oder eine Auflistung von einem dieser Typen.</span><span class="sxs-lookup"><span data-stu-id="f7235-998">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="f7235-999">Der Rückgabetyp einer Funktion kann angegeben werden, entweder mit der **Typ** Attribut der **ReturnType** (Funktion)-Element, oder mit einem der folgenden untergeordneten Elemente:</span><span class="sxs-lookup"><span data-stu-id="f7235-999">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="f7235-1000">CollectionType</span><span class="sxs-lookup"><span data-stu-id="f7235-1000">CollectionType</span></span>
-   <span data-ttu-id="f7235-1001">ReferenceType</span><span class="sxs-lookup"><span data-stu-id="f7235-1001">ReferenceType</span></span>
-   <span data-ttu-id="f7235-1002">RowType</span><span class="sxs-lookup"><span data-stu-id="f7235-1002">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="f7235-1003">Ein Modell überprüft nicht, ob es sich bei Angabe eines funktionsrückgabewerts sowohl die **Typ** Attribut der **ReturnType** (Funktion)-Element und einem der untergeordneten Elemente.</span><span class="sxs-lookup"><span data-stu-id="f7235-1003">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-1004">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-1004">Applicable Attributes</span></span>

<span data-ttu-id="f7235-1005">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **ReturnType** (Funktion)-Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-1005">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="f7235-1006">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-1006">Attribute Name</span></span> | <span data-ttu-id="f7235-1007">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-1007">Is Required</span></span> | <span data-ttu-id="f7235-1008">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-1008">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="f7235-1009">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="f7235-1009">**ReturnType**</span></span> | <span data-ttu-id="f7235-1010">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-1010">No</span></span>          | <span data-ttu-id="f7235-1011">Der von der Funktion zurückgegebene Typ.</span><span class="sxs-lookup"><span data-stu-id="f7235-1011">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="f7235-1012">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **ReturnType** (Funktion)-Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-1012">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="f7235-1013">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-1013">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-1014">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-1014">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="f7235-1015">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-1015">Example</span></span>

<span data-ttu-id="f7235-1016">Im folgenden Beispiel wird eine **Funktion** Element eine Funktion definiert, die die Anzahl der Jahre zurückgibt, ein Buch gedruckt wurde.</span><span class="sxs-lookup"><span data-stu-id="f7235-1016">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="f7235-1017">Beachten Sie, die durch der Rückgabetyp angegeben wird die **Typ** Attribut eine **ReturnType** (Funktion)-Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-1017">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="f7235-1018">ReturnType (FunctionImport)-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-1018">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="f7235-1019">Die **ReturnType** (FunctionImport)-Elements in der konzeptionellen Schemadefinitionssprache (CSDL) gibt an, den Rückgabetyp für eine Funktion, die in FunctionImport-Element definiert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-1019">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="f7235-1020">Eine Funktion, der Rückgabetyp kann auch angegeben werden, mit einem **ReturnType** Attribut.</span><span class="sxs-lookup"><span data-stu-id="f7235-1020">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="f7235-1021">Rückgabetypen können eine beliebige Auflistung von Entitätstypen, komplexen Typ oder **EdmSimpleType**,</span><span class="sxs-lookup"><span data-stu-id="f7235-1021">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="f7235-1022">Der Rückgabetyp einer Funktion wird angegeben, mit der **Typ** Attribut der **ReturnType** (FunctionImport)-Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-1022">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-1023">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-1023">Applicable Attributes</span></span>

<span data-ttu-id="f7235-1024">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **ReturnType** (FunctionImport)-Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-1024">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="f7235-1025">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-1025">Attribute Name</span></span> | <span data-ttu-id="f7235-1026">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-1026">Is Required</span></span> | <span data-ttu-id="f7235-1027">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-1027">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f7235-1028">**Type**</span><span class="sxs-lookup"><span data-stu-id="f7235-1028">**Type**</span></span>       | <span data-ttu-id="f7235-1029">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-1029">No</span></span>          | <span data-ttu-id="f7235-1030">Der Typ, den die Funktion zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="f7235-1030">The type that the function returns.</span></span> <span data-ttu-id="f7235-1031">Der Wert muss es sich um eine Auflistung von ComplexType, EntityType oder EDMSimpleType sein.</span><span class="sxs-lookup"><span data-stu-id="f7235-1031">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="f7235-1032">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="f7235-1032">**EntitySet**</span></span>  | <span data-ttu-id="f7235-1033">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-1033">No</span></span>          | <span data-ttu-id="f7235-1034">Wenn die Funktion eine Auflistung von Entitätstypen zurückgibt Typen verwenden, wird der Wert von der **EntitySet** muss der Entitätenmenge entsprechen, zu dem die Auflistung gehört.</span><span class="sxs-lookup"><span data-stu-id="f7235-1034">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="f7235-1035">Andernfalls die **EntitySet** Attribut darf nicht verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="f7235-1035">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="f7235-1036">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **ReturnType** (FunctionImport)-Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-1036">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="f7235-1037">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-1037">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-1038">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-1038">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="f7235-1039">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-1039">Example</span></span>

<span data-ttu-id="f7235-1040">Im folgenden Beispiel wird eine **FunctionImport** , Bücher und Verleger zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="f7235-1040">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="f7235-1041">Beachten Sie, dass die Funktion, zwei Resultsets aus, und daher zwei zurückgibt **ReturnType** (FunctionImport)-Element angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-1041">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="f7235-1042">RowType-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-1042">RowType Element (CSDL)</span></span>

<span data-ttu-id="f7235-1043">Ein **RowType** -Element in konzeptioneller Schemadefinitionssprache (CSDL) definiert eine unbenannte Struktur als Parameter oder Rückgabetyp für eine Funktion im konzeptionellen Modell definiert.</span><span class="sxs-lookup"><span data-stu-id="f7235-1043">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="f7235-1044">Ein **RowType** Element kann das untergeordnete Element in der folgenden Elemente sein:</span><span class="sxs-lookup"><span data-stu-id="f7235-1044">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="f7235-1045">CollectionType</span><span class="sxs-lookup"><span data-stu-id="f7235-1045">CollectionType</span></span>
-   <span data-ttu-id="f7235-1046">Parameter</span><span class="sxs-lookup"><span data-stu-id="f7235-1046">Parameter</span></span>
-   <span data-ttu-id="f7235-1047">ReturnType (Funktion)</span><span class="sxs-lookup"><span data-stu-id="f7235-1047">ReturnType (Function)</span></span>

<span data-ttu-id="f7235-1048">Ein **RowType** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="f7235-1048">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f7235-1049">Eigenschaft (ein oder mehrere)</span><span class="sxs-lookup"><span data-stu-id="f7235-1049">Property (one or more)</span></span>
-   <span data-ttu-id="f7235-1050">Anmerkungselemente (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="f7235-1050">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-1051">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-1051">Applicable Attributes</span></span>

<span data-ttu-id="f7235-1052">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **RowType** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-1052">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="f7235-1053">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-1053">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-1054">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-1054">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="f7235-1055">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-1055">Example</span></span>

<span data-ttu-id="f7235-1056">Das folgende Beispiel zeigt eine modelldefinierte Funktion, verwendet eine **CollectionType** Element, um anzugeben, dass die Funktion eine Auflistung von Zeilen zurückgibt (gemäß den Angaben in der **RowType** Element).</span><span class="sxs-lookup"><span data-stu-id="f7235-1056">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

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

## <a name="schema-element-csdl"></a><span data-ttu-id="f7235-1057">Schema-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-1057">Schema Element (CSDL)</span></span>

<span data-ttu-id="f7235-1058">Die **Schema** Element ist das Stammelement einer konzeptionellen Modelldefinition.</span><span class="sxs-lookup"><span data-stu-id="f7235-1058">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="f7235-1059">Es enthält Definitionen für die Objekte, Funktionen und Container, die ein konzeptionelles Modell bilden.</span><span class="sxs-lookup"><span data-stu-id="f7235-1059">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="f7235-1060">Die **Schema** -Element kann 0 (null) oder mehrere der folgenden untergeordneten Elemente enthalten:</span><span class="sxs-lookup"><span data-stu-id="f7235-1060">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="f7235-1061">Using</span><span class="sxs-lookup"><span data-stu-id="f7235-1061">Using</span></span>
-   <span data-ttu-id="f7235-1062">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="f7235-1062">EntityContainer</span></span>
-   <span data-ttu-id="f7235-1063">EntityType</span><span class="sxs-lookup"><span data-stu-id="f7235-1063">EntityType</span></span>
-   <span data-ttu-id="f7235-1064">EnumType</span><span class="sxs-lookup"><span data-stu-id="f7235-1064">EnumType</span></span>
-   <span data-ttu-id="f7235-1065">Zuordnung</span><span class="sxs-lookup"><span data-stu-id="f7235-1065">Association</span></span>
-   <span data-ttu-id="f7235-1066">ComplexType</span><span class="sxs-lookup"><span data-stu-id="f7235-1066">ComplexType</span></span>
-   <span data-ttu-id="f7235-1067">Funktion</span><span class="sxs-lookup"><span data-stu-id="f7235-1067">Function</span></span>

<span data-ttu-id="f7235-1068">Ein **Schema** Element darf keinem oder einem Anmerkungselemente enthalten.</span><span class="sxs-lookup"><span data-stu-id="f7235-1068">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="f7235-1069">Die **Funktion** -Element und Anmerkungselemente sind nur zulässig, in der CSDL-v2 und höher.</span><span class="sxs-lookup"><span data-stu-id="f7235-1069">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="f7235-1070">Die **Schema** -Element verwendet die **Namespace** Attribut, um den Namespace für den Entitätstyp, den komplexen Typ und die Zuordnungsobjekte in einem konzeptionellen Modell zu definieren.</span><span class="sxs-lookup"><span data-stu-id="f7235-1070">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="f7235-1071">Innerhalb eines Namespace müssen alle Objekte eine eindeutige Bezeichnung aufweisen.</span><span class="sxs-lookup"><span data-stu-id="f7235-1071">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="f7235-1072">Namespaces können mehrere umfassen **Schema** Elemente und mehrere CSDL-Dateien.</span><span class="sxs-lookup"><span data-stu-id="f7235-1072">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="f7235-1073">Namespace in einem konzeptionellen Modell unterscheidet sich von den XML-Namespace die **Schema** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-1073">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="f7235-1074">Namespace in einem konzeptionellen Modell (gemäß der **Namespace** Attribut) ist ein logischer Container für Entitätstypen, komplexe Typen und Zuordnungstypen.</span><span class="sxs-lookup"><span data-stu-id="f7235-1074">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="f7235-1075">Der XML-Namespace (angegeben durch die **Xmlns** Attribut) des eine **Schema** Element ist der Standardnamespace für untergeordnete Elemente und Attribute des der **Schema** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-1075">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="f7235-1076">XML-Namespaces im Format http://schemas.microsoft.com/ado/YYYY/MM/edm (, wobei YYYY und MM ein Jahr und Monat darstellen) sind für CSDL reserviert.</span><span class="sxs-lookup"><span data-stu-id="f7235-1076">XML namespaces of the form http://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="f7235-1077">Benutzerdefinierte Elemente und Attribute können nicht in Namespaces mit diesem Format vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="f7235-1077">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-1078">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-1078">Applicable Attributes</span></span>

<span data-ttu-id="f7235-1079">Die folgende Tabelle beschreibt die Attribute angewendet werden können, um die **Schema** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-1079">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="f7235-1080">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-1080">Attribute Name</span></span> | <span data-ttu-id="f7235-1081">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-1081">Is Required</span></span> | <span data-ttu-id="f7235-1082">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-1082">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f7235-1083">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="f7235-1083">**Namespace**</span></span>  | <span data-ttu-id="f7235-1084">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-1084">Yes</span></span>         | <span data-ttu-id="f7235-1085">Der Namespace für das konzeptionelle Modell.</span><span class="sxs-lookup"><span data-stu-id="f7235-1085">The namespace of the conceptual model.</span></span> <span data-ttu-id="f7235-1086">Der Wert des der **Namespace** Attribut wird verwendet, um den vollqualifizierten Namen eines Typs zu bilden.</span><span class="sxs-lookup"><span data-stu-id="f7235-1086">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="f7235-1087">Z. B. wenn ein **EntityType** mit dem Namen *Kunden* befindet sich im Simple.Example.Model-Namespace, und klicken Sie dann auf den vollqualifizierten Namen des der **EntityType** ist SimpleExampleModel.Customer.</span><span class="sxs-lookup"><span data-stu-id="f7235-1087">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="f7235-1088">Die folgenden Zeichenfolgen können nicht verwendet werden, als Wert für die **Namespace** Attribut: **System**, **vorübergehende**, oder **Edm**.</span><span class="sxs-lookup"><span data-stu-id="f7235-1088">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="f7235-1089">Der Wert für die **Namespace** Attribut kann nicht übereinstimmen, den der Wert für die **Namespace** Attribut im SSDL-Schema-Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-1089">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="f7235-1090">**Alias**</span><span class="sxs-lookup"><span data-stu-id="f7235-1090">**Alias**</span></span>      | <span data-ttu-id="f7235-1091">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-1091">No</span></span>          | <span data-ttu-id="f7235-1092">Ein anstelle der Namespacebezeichnung verwendeter Bezeichner.</span><span class="sxs-lookup"><span data-stu-id="f7235-1092">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="f7235-1093">Z. B. wenn ein **EntityType** mit dem Namen *Kunden* befindet sich im Simple.Example.Model-Namespace und den Wert des der **Alias** Attribut *Modell*, dann kann "Modell.Kunde" als den vollqualifizierten Namen der können Sie die **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="f7235-1093">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="f7235-1094">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Schema** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-1094">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="f7235-1095">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-1095">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-1096">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-1096">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="f7235-1097">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-1097">Example</span></span>

<span data-ttu-id="f7235-1098">Das folgende Beispiel zeigt eine **Schema** -Element mit einem **EntityContainer** Element, zwei **EntityType** -Elemente und ein **Zuordnung** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-1098">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
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
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="f7235-1099">TypeRef-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-1099">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="f7235-1100">Die **TypeRef** -Element in konzeptioneller Schemadefinitionssprache (CSDL) stellt einen Verweis auf einen vorhandenen benannten Typ bereit.</span><span class="sxs-lookup"><span data-stu-id="f7235-1100">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="f7235-1101">Die **TypeRef** Element kann sein, ein untergeordnetes Element des CollectionType-Element, das verwendet wird, um anzugeben, dass eine Funktion eine Auflistung als ein Parameter oder Rückgabetyp verfügt.</span><span class="sxs-lookup"><span data-stu-id="f7235-1101">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="f7235-1102">Ein **TypeRef** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="f7235-1102">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f7235-1103">Dokumentation (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="f7235-1103">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="f7235-1104">Anmerkungselemente (null oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="f7235-1104">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-1105">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-1105">Applicable Attributes</span></span>

<span data-ttu-id="f7235-1106">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **TypeRef** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-1106">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="f7235-1107">Beachten Sie, dass die **DefaultValue**, **MaxLength**, **FixedLength**, **Genauigkeit**, **Skalierung**,  **Unicode**, und **Sortierreihenfolge** Attribute gelten nur für **abzielt**.</span><span class="sxs-lookup"><span data-stu-id="f7235-1107">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="f7235-1108">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-1108">Attribute Name</span></span>                                                     | <span data-ttu-id="f7235-1109">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-1109">Is Required</span></span> | <span data-ttu-id="f7235-1110">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-1110">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f7235-1111">**Type**</span><span class="sxs-lookup"><span data-stu-id="f7235-1111">**Type**</span></span>                                                           | <span data-ttu-id="f7235-1112">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-1112">No</span></span>          | <span data-ttu-id="f7235-1113">Der Name der Typbibliothek, auf die verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-1113">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="f7235-1114">**NULL-Werte zulässt**</span><span class="sxs-lookup"><span data-stu-id="f7235-1114">**Nullable**</span></span>                                                       | <span data-ttu-id="f7235-1115">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-1115">No</span></span>          | <span data-ttu-id="f7235-1116">**"True"** (Standardwert) oder **"false"** abhängig davon, ob die Eigenschaft einen null-Wert haben kann.</span><span class="sxs-lookup"><span data-stu-id="f7235-1116">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="f7235-1117">> In CSDL v1 Eigenschaft eines komplexen Typs benötigen `Nullable="False"`.</span><span class="sxs-lookup"><span data-stu-id="f7235-1117">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="f7235-1118">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="f7235-1118">**DefaultValue**</span></span>                                                   | <span data-ttu-id="f7235-1119">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-1119">No</span></span>          | <span data-ttu-id="f7235-1120">Der Standardwert der Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="f7235-1120">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="f7235-1121">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="f7235-1121">**MaxLength**</span></span>                                                      | <span data-ttu-id="f7235-1122">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-1122">No</span></span>          | <span data-ttu-id="f7235-1123">Die maximale Länge des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="f7235-1123">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="f7235-1124">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="f7235-1124">**FixedLength**</span></span>                                                    | <span data-ttu-id="f7235-1125">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-1125">No</span></span>          | <span data-ttu-id="f7235-1126">**"True"** oder **"false"** abhängig davon, ob der Eigenschaftswert als Zeichenfolge mit fester Länge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-1126">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="f7235-1127">**Genauigkeit**</span><span class="sxs-lookup"><span data-stu-id="f7235-1127">**Precision**</span></span>                                                      | <span data-ttu-id="f7235-1128">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-1128">No</span></span>          | <span data-ttu-id="f7235-1129">Die Genauigkeit des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="f7235-1129">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="f7235-1130">**Scale** (Skalieren)</span><span class="sxs-lookup"><span data-stu-id="f7235-1130">**Scale**</span></span>                                                          | <span data-ttu-id="f7235-1131">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-1131">No</span></span>          | <span data-ttu-id="f7235-1132">Die Skalierung des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="f7235-1132">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="f7235-1133">**SRID**</span><span class="sxs-lookup"><span data-stu-id="f7235-1133">**SRID**</span></span>                                                           | <span data-ttu-id="f7235-1134">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-1134">No</span></span>          | <span data-ttu-id="f7235-1135">Räumliche Verweis Systembezeichner.</span><span class="sxs-lookup"><span data-stu-id="f7235-1135">Spatial System Reference Identifier.</span></span> <span data-ttu-id="f7235-1136">Gilt nur für Eigenschaften des räumlichen Typen.</span><span class="sxs-lookup"><span data-stu-id="f7235-1136">Valid only for properties of spatial types.</span></span> <span data-ttu-id="f7235-1137">Weitere Informationen finden Sie unter [SRID](http://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="f7235-1137">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="f7235-1138">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="f7235-1138">**Unicode**</span></span>                                                        | <span data-ttu-id="f7235-1139">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-1139">No</span></span>          | <span data-ttu-id="f7235-1140">**"True"** oder **"false"** abhängig davon, ob der Eigenschaftswert als Unicode-Zeichenfolge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-1140">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="f7235-1141">**Sortierung**</span><span class="sxs-lookup"><span data-stu-id="f7235-1141">**Collation**</span></span>                                                      | <span data-ttu-id="f7235-1142">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-1142">No</span></span>          | <span data-ttu-id="f7235-1143">Eine Zeichenfolge, die die Sortierreihenfolge angibt, die in der Datenquelle verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="f7235-1143">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="f7235-1144">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **CollectionType** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-1144">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="f7235-1145">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-1145">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-1146">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-1146">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="f7235-1147">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-1147">Example</span></span>

<span data-ttu-id="f7235-1148">Das folgende Beispiel zeigt eine modelldefinierte Funktion, verwendet der **TypeRef** Element (als untergeordnetes Element eine **CollectionType** Element) angeben, dass die Funktion eine Auflistung von akzeptiert  **Abteilung** Entitätstypen.</span><span class="sxs-lookup"><span data-stu-id="f7235-1148">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

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
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="f7235-1149">Using-Element (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-1149">Using Element (CSDL)</span></span>

<span data-ttu-id="f7235-1150">Die **Using** -Element in konzeptioneller Schemadefinitionssprache (CSDL) importiert den Inhalt eines konzeptionellen Modells, das in einem anderen Namespace vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-1150">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="f7235-1151">Durch Festlegen des Werts, der die **Namespace** Attribut Sie verweisen auf Entitätstypen, komplexe Typen und Zuordnungstypen, die in einem anderen konzeptionellen Modell definiert sind.</span><span class="sxs-lookup"><span data-stu-id="f7235-1151">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="f7235-1152">Mehr als eine **Using** Element ein untergeordnetes Element des kann sein, eine **Schema** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-1152">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="f7235-1153">Die **verwenden** -Element in CSDL funktioniert nicht genau so wie eine **mit** -Anweisung in einer Programmiersprache.</span><span class="sxs-lookup"><span data-stu-id="f7235-1153">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="f7235-1154">Durch Importieren eines Namespace mit einem **mit** Anweisung in einer Programmiersprache, Sie wirken sich nicht die Objekte im ursprünglichen Namespace.</span><span class="sxs-lookup"><span data-stu-id="f7235-1154">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="f7235-1155">In CSDL kann ein importierter Namespace einen Entitätstyp enthalten, der von einem Entitätstyp im ursprünglichen Namespace abgeleitet ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-1155">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="f7235-1156">Dies kann sich auf im ursprünglichen Namespace deklarierte Entitätssätze auswirken.</span><span class="sxs-lookup"><span data-stu-id="f7235-1156">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="f7235-1157">Die **Using** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="f7235-1157">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="f7235-1158">Dokumentation (0 (null) oder ein Element zugelassen)</span><span class="sxs-lookup"><span data-stu-id="f7235-1158">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="f7235-1159">Anmerkungselemente (0 (null) oder mehrere Elemente zugelassen)</span><span class="sxs-lookup"><span data-stu-id="f7235-1159">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f7235-1160">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="f7235-1160">Applicable Attributes</span></span>

<span data-ttu-id="f7235-1161">Die folgende Tabelle beschreibt die Attribute angewendet werden können, um die **verwenden** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-1161">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="f7235-1162">Attributname</span><span class="sxs-lookup"><span data-stu-id="f7235-1162">Attribute Name</span></span> | <span data-ttu-id="f7235-1163">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="f7235-1163">Is Required</span></span> | <span data-ttu-id="f7235-1164">Wert</span><span class="sxs-lookup"><span data-stu-id="f7235-1164">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f7235-1165">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="f7235-1165">**Namespace**</span></span>  | <span data-ttu-id="f7235-1166">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-1166">Yes</span></span>         | <span data-ttu-id="f7235-1167">Der Name des importierten Namespaces.</span><span class="sxs-lookup"><span data-stu-id="f7235-1167">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="f7235-1168">**Alias**</span><span class="sxs-lookup"><span data-stu-id="f7235-1168">**Alias**</span></span>      | <span data-ttu-id="f7235-1169">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-1169">Yes</span></span>         | <span data-ttu-id="f7235-1170">Ein anstelle der Namespacebezeichnung verwendeter Bezeichner.</span><span class="sxs-lookup"><span data-stu-id="f7235-1170">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="f7235-1171">Obwohl dieses Attribut erforderlich ist, muss es nicht anstelle des Namespacenamens verwendet wird, um Objektnamen zu qualifizieren.</span><span class="sxs-lookup"><span data-stu-id="f7235-1171">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="f7235-1172">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Using** Element.</span><span class="sxs-lookup"><span data-stu-id="f7235-1172">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="f7235-1173">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-1173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="f7235-1174">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-1174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="f7235-1175">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-1175">Example</span></span>

<span data-ttu-id="f7235-1176">Das folgende Beispiel zeigt die **Using** Element verwendet wird, um einen Namespace zu importieren, die an anderer Stelle, definiert.</span><span class="sxs-lookup"><span data-stu-id="f7235-1176">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="f7235-1177">Beachten Sie, dass der Namespace für die **Schema** Element ist das `BooksModel`.</span><span class="sxs-lookup"><span data-stu-id="f7235-1177">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="f7235-1178">Die `Address` Eigenschaft für die `Publisher` **EntityType** ist ein komplexer Typ, der in definiert ist die `ExtendedBooksModel` Namespace (mit importiert die **verwenden** Element).</span><span class="sxs-lookup"><span data-stu-id="f7235-1178">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

``` xml
 <Schema xmlns="http://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="http://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="http://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
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
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="f7235-1179">Anmerkungsattribute (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-1179">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="f7235-1180">Anmerkungsattribute in konzeptioneller Schemadefinitionssprache (CSDL) sind benutzerdefinierte XML-Attribute im konzeptionellen Modell.</span><span class="sxs-lookup"><span data-stu-id="f7235-1180">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="f7235-1181">Neben dem Vorhandensein einer gültigen XML-Struktur muss für Anmerkungsattribute folgendes zutreffen:</span><span class="sxs-lookup"><span data-stu-id="f7235-1181">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="f7235-1182">Anmerkungsattribute dürfen sich in keinem XML-Namespace befinden, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-1182">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="f7235-1183">Für ein angegebenes CSDL-Element kann mehr als ein Anmerkungsattribut übernommen werden.</span><span class="sxs-lookup"><span data-stu-id="f7235-1183">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="f7235-1184">Die vollqualifizierten Namen zweier Anmerkungsattribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-1184">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="f7235-1185">Anmerkungsattribute können verwendet werden, um zusätzliche Metadaten für die Elemente in einem konzeptionellen Modell bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="f7235-1185">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="f7235-1186">Metadaten in Anmerkungselementen kann zur Laufzeit zugegriffen werden, mithilfe von Klassen im Namespace System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="f7235-1186">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="f7235-1187">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-1187">Example</span></span>

<span data-ttu-id="f7235-1188">Das folgende Beispiel zeigt eine **EntityType** Element mit einem anmerkungsattribut (**CustomAttribute**).</span><span class="sxs-lookup"><span data-stu-id="f7235-1188">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="f7235-1189">Das Beispiel zeigt außerdem ein Anmerkungselement, das auf das Entitätstypelement angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-1189">The example also shows an annotation element applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
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
 

<span data-ttu-id="f7235-1190">Im folgenden Code werden die Metadaten im Anmerkungsattribut abgerufen und in die Konsole geschrieben:</span><span class="sxs-lookup"><span data-stu-id="f7235-1190">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

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
 

<span data-ttu-id="f7235-1191">Im Code oben wird davon ausgegangen, dass sich die Datei `School.csdl` im Ausgabeverzeichnis des Projekts befindet und dass Sie dem Projekt die folgenden `Imports`- und `Using`-Anweisungen hinzugefügt haben:</span><span class="sxs-lookup"><span data-stu-id="f7235-1191">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="f7235-1192">Anmerkungselemente (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-1192">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="f7235-1193">Anmerkungselemente in konzeptioneller Schemadefinitionssprache (CSDL) sind benutzerdefinierte XML-Elemente im konzeptionellen Modell.</span><span class="sxs-lookup"><span data-stu-id="f7235-1193">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="f7235-1194">Neben dem Vorhandensein einer gültigen XML-Struktur muss für Anmerkungselemente Folgendes zutreffen:</span><span class="sxs-lookup"><span data-stu-id="f7235-1194">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="f7235-1195">Anmerkungselemente dürfen sich in keinem XML-Namespace befinden, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="f7235-1195">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="f7235-1196">Mehrere Anmerkungselemente können untergeordnete Elemente eines angegebenen CSDL-Elements sein.</span><span class="sxs-lookup"><span data-stu-id="f7235-1196">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="f7235-1197">Die vollqualifizierten Namen zweier Anmerkungselemente dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="f7235-1197">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="f7235-1198">Anmerkungselemente müssen nach allen anderen untergeordneten Elementen eines angegebenen CSDL-Elements angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="f7235-1198">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="f7235-1199">Anmerkungselemente können verwendet werden, um zusätzliche Metadaten für die Elemente in einem konzeptionellen Modell bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="f7235-1199">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="f7235-1200">Ab .NET Framework, Version 4, kann in Anmerkungselementen enthaltene Metadaten zur Laufzeit zugegriffen werden mithilfe von Klassen im Namespace System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="f7235-1200">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="f7235-1201">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-1201">Example</span></span>

<span data-ttu-id="f7235-1202">Das folgende Beispiel zeigt eine **EntityType** Element mit einem Anmerkungselement (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="f7235-1202">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="f7235-1203">Das Beispiel zeigt außerdem ein Anmerkungsattribut, das auf das Entitätstypelement angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-1203">The example also show an annotation attribute applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="http://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="http://schemas.microsoft.com/ado/2009/11/edm">
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
 

<span data-ttu-id="f7235-1204">Im folgenden Code werden die Metadaten im Anmerkungselement abgerufen und in die Konsole geschrieben:</span><span class="sxs-lookup"><span data-stu-id="f7235-1204">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

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
 

<span data-ttu-id="f7235-1205">Im Code oben wird davon ausgegangen, dass sich die Datei School.csdl im Ausgabeverzeichnis des Projekts befindet, und dass Sie dem Projekt die folgenden `Imports`- und `Using`-Anweisungen hinzugefügt haben:</span><span class="sxs-lookup"><span data-stu-id="f7235-1205">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="f7235-1206">Konzeptionelle Modelltypen (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-1206">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="f7235-1207">Konzeptionelle Schemadefinitionssprache (CSDL) unterstützt einen Satz von abstrakten primitiven Datentypen mit dem Namen **abzielt**, definieren Sie Eigenschaften, die in einem konzeptionellen Modell.</span><span class="sxs-lookup"><span data-stu-id="f7235-1207">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="f7235-1208">**Abzielt** sind Proxys für primitive Datentypen, die in der Speicher- oder Hostingumgebung unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="f7235-1208">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="f7235-1209">In der nachfolgenden Tabelle werden die von CSDL unterstützten primitiven Datentypen aufgeführt.</span><span class="sxs-lookup"><span data-stu-id="f7235-1209">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="f7235-1210">Die Tabelle enthält außerdem die Facets, die auf die einzelnen angewendet werden können **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="f7235-1210">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="f7235-1211">EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="f7235-1211">EDMSimpleType</span></span>                    | <span data-ttu-id="f7235-1212">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="f7235-1212">Description</span></span>                                                | <span data-ttu-id="f7235-1213">Anwendbare Facets</span><span class="sxs-lookup"><span data-stu-id="f7235-1213">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="f7235-1214">**Edm.Binary**</span><span class="sxs-lookup"><span data-stu-id="f7235-1214">**Edm.Binary**</span></span>                   | <span data-ttu-id="f7235-1215">Enthält Binärdaten.</span><span class="sxs-lookup"><span data-stu-id="f7235-1215">Contains binary data.</span></span>                                      | <span data-ttu-id="f7235-1216">MaxLength, FixedLength, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="f7235-1216">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="f7235-1217">**Edm.Boolean**</span><span class="sxs-lookup"><span data-stu-id="f7235-1217">**Edm.Boolean**</span></span>                  | <span data-ttu-id="f7235-1218">Enthält den Wert **"true"** oder **"false"**.</span><span class="sxs-lookup"><span data-stu-id="f7235-1218">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="f7235-1219">Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="f7235-1219">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="f7235-1220">**Edm.Byte**</span><span class="sxs-lookup"><span data-stu-id="f7235-1220">**Edm.Byte**</span></span>                     | <span data-ttu-id="f7235-1221">Enthält einen 8-Bit-Ganzzahlwert ohne Vorzeichen.</span><span class="sxs-lookup"><span data-stu-id="f7235-1221">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="f7235-1222">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="f7235-1222">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="f7235-1223">**Edm.DateTime**</span><span class="sxs-lookup"><span data-stu-id="f7235-1223">**Edm.DateTime**</span></span>                 | <span data-ttu-id="f7235-1224">Stellt ein Datum und eine Uhrzeit dar.</span><span class="sxs-lookup"><span data-stu-id="f7235-1224">Represents a date and time.</span></span>                                | <span data-ttu-id="f7235-1225">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="f7235-1225">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="f7235-1226">**Edm.DateTimeOffset**</span><span class="sxs-lookup"><span data-stu-id="f7235-1226">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="f7235-1227">Enthält ein Datum und eine Uhrzeit als Offset in Minuten von GMT.</span><span class="sxs-lookup"><span data-stu-id="f7235-1227">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="f7235-1228">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="f7235-1228">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="f7235-1229">**Edm.Decimal**</span><span class="sxs-lookup"><span data-stu-id="f7235-1229">**Edm.Decimal**</span></span>                  | <span data-ttu-id="f7235-1230">Enthält einen numerischen Wert mit fester Genauigkeit und festen Dezimalstellen.</span><span class="sxs-lookup"><span data-stu-id="f7235-1230">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="f7235-1231">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="f7235-1231">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="f7235-1232">**"Edm.Double"**</span><span class="sxs-lookup"><span data-stu-id="f7235-1232">**Edm.Double**</span></span>                   | <span data-ttu-id="f7235-1233">Enthält eine Gleitkommazahl mit 15 Stellen Genauigkeit</span><span class="sxs-lookup"><span data-stu-id="f7235-1233">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="f7235-1234">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="f7235-1234">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="f7235-1235">**Edm.Float**</span><span class="sxs-lookup"><span data-stu-id="f7235-1235">**Edm.Float**</span></span>                    | <span data-ttu-id="f7235-1236">Enthält eine Gleitkommazahl mit einer Genauigkeit von 7 Stellen.</span><span class="sxs-lookup"><span data-stu-id="f7235-1236">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="f7235-1237">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="f7235-1237">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="f7235-1238">**Edm.Guid**</span><span class="sxs-lookup"><span data-stu-id="f7235-1238">**Edm.Guid**</span></span>                     | <span data-ttu-id="f7235-1239">Enthält einen eindeutigen 16-Byte-Bezeichner.</span><span class="sxs-lookup"><span data-stu-id="f7235-1239">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="f7235-1240">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="f7235-1240">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="f7235-1241">**Edm.Int16**</span><span class="sxs-lookup"><span data-stu-id="f7235-1241">**Edm.Int16**</span></span>                    | <span data-ttu-id="f7235-1242">Enthält einen 16-Bit-Ganzzahlwert mit Vorzeichen.</span><span class="sxs-lookup"><span data-stu-id="f7235-1242">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="f7235-1243">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="f7235-1243">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="f7235-1244">**Int32**</span><span class="sxs-lookup"><span data-stu-id="f7235-1244">**Edm.Int32**</span></span>                    | <span data-ttu-id="f7235-1245">Enthält einen 32-Bit-Ganzzahlwert mit Vorzeichen.</span><span class="sxs-lookup"><span data-stu-id="f7235-1245">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="f7235-1246">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="f7235-1246">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="f7235-1247">**Int64**</span><span class="sxs-lookup"><span data-stu-id="f7235-1247">**Edm.Int64**</span></span>                    | <span data-ttu-id="f7235-1248">Enthält einen 64-Bit-Ganzzahlwert mit Vorzeichen.</span><span class="sxs-lookup"><span data-stu-id="f7235-1248">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="f7235-1249">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="f7235-1249">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="f7235-1250">**Edm.SByte**</span><span class="sxs-lookup"><span data-stu-id="f7235-1250">**Edm.SByte**</span></span>                    | <span data-ttu-id="f7235-1251">Enthält einen 8-Bit-Ganzzahlwert mit Vorzeichen.</span><span class="sxs-lookup"><span data-stu-id="f7235-1251">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="f7235-1252">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="f7235-1252">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="f7235-1253">**"Edm.String"**</span><span class="sxs-lookup"><span data-stu-id="f7235-1253">**Edm.String**</span></span>                   | <span data-ttu-id="f7235-1254">Enthält Zeichendaten.</span><span class="sxs-lookup"><span data-stu-id="f7235-1254">Contains character data.</span></span>                                   | <span data-ttu-id="f7235-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="f7235-1255">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="f7235-1256">**Edm.Time**</span><span class="sxs-lookup"><span data-stu-id="f7235-1256">**Edm.Time**</span></span>                     | <span data-ttu-id="f7235-1257">Enthält eine Uhrzeit.</span><span class="sxs-lookup"><span data-stu-id="f7235-1257">Contains a time of day.</span></span>                                    | <span data-ttu-id="f7235-1258">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="f7235-1258">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="f7235-1259">**Edm.Geography**</span><span class="sxs-lookup"><span data-stu-id="f7235-1259">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="f7235-1260">NULL-Werte zulässt, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="f7235-1260">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="f7235-1261">**"Edm.geographypoint"**</span><span class="sxs-lookup"><span data-stu-id="f7235-1261">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="f7235-1262">NULL-Werte zulässt, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="f7235-1262">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="f7235-1263">**Edm.GeographyLineString**</span><span class="sxs-lookup"><span data-stu-id="f7235-1263">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="f7235-1264">NULL-Werte zulässt, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="f7235-1264">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="f7235-1265">**Edm.GeographyPolygon**</span><span class="sxs-lookup"><span data-stu-id="f7235-1265">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="f7235-1266">NULL-Werte zulässt, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="f7235-1266">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="f7235-1267">**Edm.GeographyMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="f7235-1267">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="f7235-1268">NULL-Werte zulässt, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="f7235-1268">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="f7235-1269">**Edm.GeographyMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="f7235-1269">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="f7235-1270">NULL-Werte zulässt, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="f7235-1270">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="f7235-1271">**Edm.GeographyMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="f7235-1271">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="f7235-1272">NULL-Werte zulässt, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="f7235-1272">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="f7235-1273">**Edm.GeographyCollection**</span><span class="sxs-lookup"><span data-stu-id="f7235-1273">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="f7235-1274">NULL-Werte zulässt, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="f7235-1274">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="f7235-1275">**Edm.Geometry**</span><span class="sxs-lookup"><span data-stu-id="f7235-1275">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="f7235-1276">NULL-Werte zulässt, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="f7235-1276">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="f7235-1277">**Edm.GeometryPoint**</span><span class="sxs-lookup"><span data-stu-id="f7235-1277">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="f7235-1278">NULL-Werte zulässt, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="f7235-1278">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="f7235-1279">**Edm.GeometryLineString**</span><span class="sxs-lookup"><span data-stu-id="f7235-1279">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="f7235-1280">NULL-Werte zulässt, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="f7235-1280">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="f7235-1281">**Edm.GeometryPolygon**</span><span class="sxs-lookup"><span data-stu-id="f7235-1281">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="f7235-1282">NULL-Werte zulässt, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="f7235-1282">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="f7235-1283">**Edm.GeometryMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="f7235-1283">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="f7235-1284">NULL-Werte zulässt, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="f7235-1284">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="f7235-1285">**Edm.GeometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="f7235-1285">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="f7235-1286">NULL-Werte zulässt, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="f7235-1286">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="f7235-1287">**Edm.GeometryMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="f7235-1287">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="f7235-1288">NULL-Werte zulässt, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="f7235-1288">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="f7235-1289">**Edm.GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="f7235-1289">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="f7235-1290">NULL-Werte zulässt, Standard, SRID</span><span class="sxs-lookup"><span data-stu-id="f7235-1290">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="f7235-1291">Facets (CSDL)</span><span class="sxs-lookup"><span data-stu-id="f7235-1291">Facets (CSDL)</span></span>

<span data-ttu-id="f7235-1292">Facets in konzeptioneller Schemadefinitionssprache (CSDL) stellen Einschränkungen für Eigenschaften von Entitätstypen und komplexen Typen dar.</span><span class="sxs-lookup"><span data-stu-id="f7235-1292">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="f7235-1293">Facets werden in den folgenden CSDL-Elementen als XML-Attribute angezeigt:</span><span class="sxs-lookup"><span data-stu-id="f7235-1293">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="f7235-1294">Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="f7235-1294">Property</span></span>
-   <span data-ttu-id="f7235-1295">TypeRef</span><span class="sxs-lookup"><span data-stu-id="f7235-1295">TypeRef</span></span>
-   <span data-ttu-id="f7235-1296">Parameter</span><span class="sxs-lookup"><span data-stu-id="f7235-1296">Parameter</span></span>

<span data-ttu-id="f7235-1297">In der folgenden Tabelle werden die in CSDL unterstützten Facets beschrieben.</span><span class="sxs-lookup"><span data-stu-id="f7235-1297">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="f7235-1298">Alle Facets sind optional.</span><span class="sxs-lookup"><span data-stu-id="f7235-1298">All facets are optional.</span></span> <span data-ttu-id="f7235-1299">Beim Generieren einer Datenbank aus einem konzeptionellen Modell, werden einige der unten aufgeführten Facets vom Entity Framework verwendet.</span><span class="sxs-lookup"><span data-stu-id="f7235-1299">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="f7235-1300">Informationen zu Datentypen in einem konzeptionellen Modell finden Sie konzeptionelle Modelltypen (CSDL).</span><span class="sxs-lookup"><span data-stu-id="f7235-1300">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="f7235-1301">Facet</span><span class="sxs-lookup"><span data-stu-id="f7235-1301">Facet</span></span>               | <span data-ttu-id="f7235-1302">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="f7235-1302">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="f7235-1303">Betrifft</span><span class="sxs-lookup"><span data-stu-id="f7235-1303">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="f7235-1304">Wird für die Datenbankgenerierung verwendet</span><span class="sxs-lookup"><span data-stu-id="f7235-1304">Used for the database generation</span></span> | <span data-ttu-id="f7235-1305">Wird von der Laufzeit verwendet</span><span class="sxs-lookup"><span data-stu-id="f7235-1305">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="f7235-1306">**Sortierung**</span><span class="sxs-lookup"><span data-stu-id="f7235-1306">**Collation**</span></span>       | <span data-ttu-id="f7235-1307">Gibt die bei Vergleich- und Sortiervorgängen zu verwendende Sortierreihenfolge für die Werte der Eigenschaft an.</span><span class="sxs-lookup"><span data-stu-id="f7235-1307">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="f7235-1308">**"Edm.String"**</span><span class="sxs-lookup"><span data-stu-id="f7235-1308">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="f7235-1309">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-1309">Yes</span></span>                              | <span data-ttu-id="f7235-1310">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-1310">No</span></span>                  |
| <span data-ttu-id="f7235-1311">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="f7235-1311">**ConcurrencyMode**</span></span> | <span data-ttu-id="f7235-1312">Gibt an, dass der Eigenschaftswert für Prüfungen der vollständigen Parallelität verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="f7235-1312">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="f7235-1313">Alle **EDMSimpleType** Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="f7235-1313">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="f7235-1314">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-1314">No</span></span>                               | <span data-ttu-id="f7235-1315">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-1315">Yes</span></span>                 |
| <span data-ttu-id="f7235-1316">**Default**</span><span class="sxs-lookup"><span data-stu-id="f7235-1316">**Default**</span></span>         | <span data-ttu-id="f7235-1317">Gibt den Standardwert der Eigenschaft an, wenn bei der Instanziierung kein Wert angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-1317">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="f7235-1318">Alle **EDMSimpleType** Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="f7235-1318">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="f7235-1319">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-1319">Yes</span></span>                              | <span data-ttu-id="f7235-1320">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-1320">Yes</span></span>                 |
| <span data-ttu-id="f7235-1321">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="f7235-1321">**FixedLength**</span></span>     | <span data-ttu-id="f7235-1322">Gibt an, ob sich die Länge des Eigenschaftswerts ändern kann.</span><span class="sxs-lookup"><span data-stu-id="f7235-1322">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="f7235-1323">**Edm.Binary**, **"Edm.String"**</span><span class="sxs-lookup"><span data-stu-id="f7235-1323">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="f7235-1324">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-1324">Yes</span></span>                              | <span data-ttu-id="f7235-1325">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-1325">No</span></span>                  |
| <span data-ttu-id="f7235-1326">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="f7235-1326">**MaxLength**</span></span>       | <span data-ttu-id="f7235-1327">Gibt die maximale Länge des Eigenschaftswerts an.</span><span class="sxs-lookup"><span data-stu-id="f7235-1327">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="f7235-1328">**Edm.Binary**, **"Edm.String"**</span><span class="sxs-lookup"><span data-stu-id="f7235-1328">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="f7235-1329">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-1329">Yes</span></span>                              | <span data-ttu-id="f7235-1330">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-1330">No</span></span>                  |
| <span data-ttu-id="f7235-1331">**NULL-Werte zulässt**</span><span class="sxs-lookup"><span data-stu-id="f7235-1331">**Nullable**</span></span>        | <span data-ttu-id="f7235-1332">Gibt an, ob die Eigenschaft haben kann eine **null** Wert.</span><span class="sxs-lookup"><span data-stu-id="f7235-1332">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="f7235-1333">Alle **EDMSimpleType** Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="f7235-1333">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="f7235-1334">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-1334">Yes</span></span>                              | <span data-ttu-id="f7235-1335">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-1335">Yes</span></span>                 |
| <span data-ttu-id="f7235-1336">**Genauigkeit**</span><span class="sxs-lookup"><span data-stu-id="f7235-1336">**Precision**</span></span>       | <span data-ttu-id="f7235-1337">Bei Eigenschaften vom Typ **Decimal**, gibt die Anzahl der Ziffern ein Eigenschaftswert verfügen kann.</span><span class="sxs-lookup"><span data-stu-id="f7235-1337">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="f7235-1338">Bei Eigenschaften vom Typ **Zeit**, **"DateTime"**, und **DateTimeOffset**, gibt die Anzahl von Ziffern für die Sekundenbruchteile des Eigenschaftswerts.</span><span class="sxs-lookup"><span data-stu-id="f7235-1338">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="f7235-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span><span class="sxs-lookup"><span data-stu-id="f7235-1339">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="f7235-1340">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-1340">Yes</span></span>                              | <span data-ttu-id="f7235-1341">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-1341">No</span></span>                  |
| <span data-ttu-id="f7235-1342">**Scale** (Skalieren)</span><span class="sxs-lookup"><span data-stu-id="f7235-1342">**Scale**</span></span>           | <span data-ttu-id="f7235-1343">Gibt die Anzahl der Dezimalstellen für den Eigenschaftswert an.</span><span class="sxs-lookup"><span data-stu-id="f7235-1343">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="f7235-1344">**Edm.Decimal**</span><span class="sxs-lookup"><span data-stu-id="f7235-1344">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="f7235-1345">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-1345">Yes</span></span>                              | <span data-ttu-id="f7235-1346">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-1346">No</span></span>                  |
| <span data-ttu-id="f7235-1347">**SRID**</span><span class="sxs-lookup"><span data-stu-id="f7235-1347">**SRID**</span></span>            | <span data-ttu-id="f7235-1348">Gibt an, die räumliche Verweis System-ID</span><span class="sxs-lookup"><span data-stu-id="f7235-1348">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="f7235-1349">Weitere Informationen finden Sie unter [SRID](http://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="f7235-1349">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="f7235-1350">**Edm.Geography, "Edm.geographypoint", Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="f7235-1350">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="f7235-1351">Nein</span><span class="sxs-lookup"><span data-stu-id="f7235-1351">No</span></span>                               | <span data-ttu-id="f7235-1352">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-1352">Yes</span></span>                 |
| <span data-ttu-id="f7235-1353">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="f7235-1353">**Unicode**</span></span>         | <span data-ttu-id="f7235-1354">Gibt an, ob der Eigenschaftswert als Unicode gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-1354">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="f7235-1355">**"Edm.String"**</span><span class="sxs-lookup"><span data-stu-id="f7235-1355">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="f7235-1356">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-1356">Yes</span></span>                              | <span data-ttu-id="f7235-1357">Ja</span><span class="sxs-lookup"><span data-stu-id="f7235-1357">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="f7235-1358">Beim Generieren einer Datenbank aus einem konzeptionellen Modell erkennt der Assistent zur Datenbankgenerierung den Wert des der **StoreGeneratedPattern** -Attribut für eine **Eigenschaft** Elements, sofern es in der folgenden Namespace: http://schemas.microsoft.com/ado/2009/02/edm/annotation.</span><span class="sxs-lookup"><span data-stu-id="f7235-1358">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: http://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="f7235-1359">Die unterstützten Werte für das Attribut sind **Identität** und **berechnet**.</span><span class="sxs-lookup"><span data-stu-id="f7235-1359">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="f7235-1360">Der Wert **Identität** erzeugt eine Datenbankspalte mit einem Identitätswert, der in der Datenbank generiert wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-1360">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="f7235-1361">Der Wert **berechnet** erzeugt eine Spalte mit einem Wert, der in der Datenbank berechnet wird.</span><span class="sxs-lookup"><span data-stu-id="f7235-1361">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="f7235-1362">Beispiel</span><span class="sxs-lookup"><span data-stu-id="f7235-1362">Example</span></span>

<span data-ttu-id="f7235-1363">Das folgende Beispiel zeigt für die Eigenschaften eines Entitätstyps übernommene Facets:</span><span class="sxs-lookup"><span data-stu-id="f7235-1363">The following example shows facets applied to the properties of an entity type:</span></span>

``` xml
 <EntityType Name="Product">
   <Key>
     <PropertyRef Name="ProductId" />
   </Key>
   <Property Type="Int32"
             Name="ProductId" Nullable="false"
             a:StoreGeneratedPattern="Identity"
    xmlns:a="http://schemas.microsoft.com/ado/2009/02/edm/annotation" />
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
