---
title: SSDL-Spezifikation - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
caps.latest.revision: 3
ms.openlocfilehash: a9977c80d9a9401afdcad2284a705bcb28790fb8
ms.sourcegitcommit: 9ae4473425c5e76337c9d032b0e5dbfedf1fcf57
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121450"
---
# <a name="ssdl-specification"></a><span data-ttu-id="78a9c-102">SSDL-Spezifikation</span><span class="sxs-lookup"><span data-stu-id="78a9c-102">SSDL Specification</span></span>
<span data-ttu-id="78a9c-103">Die Datenspeicherschema-Definitionssprache (Store Schema Definition Language, SSDL) ist eine XML-basierte Sprache, die das Speichermodell einer Entity Framework-Anwendung beschreibt.</span><span class="sxs-lookup"><span data-stu-id="78a9c-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="78a9c-104">In einer Entity Framework-Anwendung Metadaten des Speichermodells aus einer SSDL-Datei (geschrieben in SSDL) in eine Instanz von der System.Data.Metadata.Edm.StoreItemCollection geladen wird und kann mit Verwendung von Methoden in der System.Data.Metadata.Edm.MetadataWorkspace-Klasse.</span><span class="sxs-lookup"><span data-stu-id="78a9c-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="78a9c-105">Entitätsframework verwendet Metadaten des Speichermodells, um Abfragen für das konzeptionelle Modell in datenspeicherspezifische Befehle zu übersetzen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="78a9c-106">Die Entity Framework Designer (EF-Designer) speichert Informationen zum Speichermodell in einer EDMX-Datei zur Entwurfszeit.</span><span class="sxs-lookup"><span data-stu-id="78a9c-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="78a9c-107">Zum Zeitpunkt der Erstellung verwendet der Entity Designer Informationen in einer EDMX-Datei aus, um die SSDL-Datei zu erstellen, die zur Laufzeit vom Entity Framework benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="78a9c-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="78a9c-108">Die verschiedenen Versionen von SSDL werden durch XML-Namespaces unterschieden.</span><span class="sxs-lookup"><span data-stu-id="78a9c-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="78a9c-109">SSDL-Version</span><span class="sxs-lookup"><span data-stu-id="78a9c-109">SSDL Version</span></span> | <span data-ttu-id="78a9c-110">XML-Namespace</span><span class="sxs-lookup"><span data-stu-id="78a9c-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="78a9c-111">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="78a9c-111">SSDL v1</span></span>      | http://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="78a9c-112">SSDL-v2</span><span class="sxs-lookup"><span data-stu-id="78a9c-112">SSDL v2</span></span>      | http://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="78a9c-113">SSDL v3</span><span class="sxs-lookup"><span data-stu-id="78a9c-113">SSDL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="78a9c-114">Zuordnungselement (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-114">Association Element (SSDL)</span></span>

<span data-ttu-id="78a9c-115">Ein **Zuordnung** Element in der Datenspeicherschema-Definitionssprache (SSDL) gibt Tabellenspalten, die beteiligt sind, in eine foreign Key-Einschränkung in der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="78a9c-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="78a9c-116">Geben Sie zwei erforderliche untergeordnete Endelemente Tabellen an den Enden der Zuordnung und die Multiplizität an jedem Ende.</span><span class="sxs-lookup"><span data-stu-id="78a9c-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="78a9c-117">Ein optionales ReferentialConstraint-Element gibt das prinzipalende und das abhängige Ende der Zuordnung sowie die teilnehmenden Spalten an.</span><span class="sxs-lookup"><span data-stu-id="78a9c-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="78a9c-118">Wenn kein **ReferentialConstraint** -Element vorhanden ist, AssociationSetMapping-Elements muss verwendet werden, um die spaltenzuordnungen für die Zuordnung anzugeben.</span><span class="sxs-lookup"><span data-stu-id="78a9c-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="78a9c-119">Die **Zuordnung** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="78a9c-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="78a9c-120">Dokumentation (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="78a9c-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="78a9c-121">Ende (genau zwei Elemente)</span><span class="sxs-lookup"><span data-stu-id="78a9c-121">End (exactly two)</span></span>
-   <span data-ttu-id="78a9c-122">ReferentialConstraint (0 (null) oder 1)</span><span class="sxs-lookup"><span data-stu-id="78a9c-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="78a9c-123">Anmerkungselemente (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="78a9c-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="78a9c-124">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="78a9c-124">Applicable Attributes</span></span>

<span data-ttu-id="78a9c-125">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Zuordnung** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="78a9c-126">Attributname</span><span class="sxs-lookup"><span data-stu-id="78a9c-126">Attribute Name</span></span> | <span data-ttu-id="78a9c-127">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="78a9c-127">Is Required</span></span> | <span data-ttu-id="78a9c-128">Wert</span><span class="sxs-lookup"><span data-stu-id="78a9c-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="78a9c-129">**Name**</span><span class="sxs-lookup"><span data-stu-id="78a9c-129">**Name**</span></span>       | <span data-ttu-id="78a9c-130">Ja</span><span class="sxs-lookup"><span data-stu-id="78a9c-130">Yes</span></span>         | <span data-ttu-id="78a9c-131">Der Name der entsprechenden Fremdschlüsseleinschränkung in der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="78a9c-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="78a9c-132">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Zuordnung** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="78a9c-133">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="78a9c-134">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="78a9c-135">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-135">Example</span></span>

<span data-ttu-id="78a9c-136">Das folgende Beispiel zeigt eine **Zuordnung** -Element, das verwendet eine **ReferentialConstraint** Element, um die Spalten anzugeben, die Teilnahme an der **FK\_CustomerOrders**  foreign Key-Einschränkung:</span><span class="sxs-lookup"><span data-stu-id="78a9c-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="associationset-element-ssdl"></a><span data-ttu-id="78a9c-137">AssociationSet-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="78a9c-138">Die **AssociationSet** -Element in der Datenspeicherschema-Definitionssprache (SSDL) stellt eine fremdschlüsseleinschränkung zwischen zwei Tabellen in der zugrunde liegenden Datenbank dar.</span><span class="sxs-lookup"><span data-stu-id="78a9c-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="78a9c-139">Die Tabellenspalten, die in der foreign Key-Einschränkung einbezogen werden in ein Association-Elements angegeben.</span><span class="sxs-lookup"><span data-stu-id="78a9c-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="78a9c-140">Die **Zuordnung** Element, das entspricht, einer angegebenen **AssociationSet** Element wird angegeben, der **Zuordnung** Attribut der **AssociationSet**  Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="78a9c-141">SSDL-Zuordnungssätze werden CSDL-Zuordnungssätzen von einem AssociationSetMapping-Element zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="78a9c-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="78a9c-142">Wenn die CSDL-Zuordnung für eine bestimmte CSDL-Zuordnung festgelegt ist jedoch mit einem ReferentialConstraint-Element, das kein entsprechendes definiert **AssociationSetMapping** Element ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="78a9c-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="78a9c-143">In diesem Fall wenn ein **AssociationSetMapping** -Element vorhanden ist, werden von der die Zuordnungen definiert überschrieben der **ReferentialConstraint** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="78a9c-144">Die **AssociationSet** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="78a9c-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="78a9c-145">Dokumentation (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="78a9c-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="78a9c-146">Ende (0 (null) oder zwei)</span><span class="sxs-lookup"><span data-stu-id="78a9c-146">End (zero or two)</span></span>
-   <span data-ttu-id="78a9c-147">Anmerkungselemente (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="78a9c-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="78a9c-148">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="78a9c-148">Applicable Attributes</span></span>

<span data-ttu-id="78a9c-149">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **AssociationSet** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="78a9c-150">Attributname</span><span class="sxs-lookup"><span data-stu-id="78a9c-150">Attribute Name</span></span>  | <span data-ttu-id="78a9c-151">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="78a9c-151">Is Required</span></span> | <span data-ttu-id="78a9c-152">Wert</span><span class="sxs-lookup"><span data-stu-id="78a9c-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="78a9c-153">**Name**</span><span class="sxs-lookup"><span data-stu-id="78a9c-153">**Name**</span></span>        | <span data-ttu-id="78a9c-154">Ja</span><span class="sxs-lookup"><span data-stu-id="78a9c-154">Yes</span></span>         | <span data-ttu-id="78a9c-155">Der Name der Fremdschlüsseleinschränkung, die der Zuordnungssatz darstellt.</span><span class="sxs-lookup"><span data-stu-id="78a9c-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="78a9c-156">**Zuordnung**</span><span class="sxs-lookup"><span data-stu-id="78a9c-156">**Association**</span></span> | <span data-ttu-id="78a9c-157">Ja</span><span class="sxs-lookup"><span data-stu-id="78a9c-157">Yes</span></span>         | <span data-ttu-id="78a9c-158">Der Name der Zuordnung, die die Spalten definiert, die an der Fremdschlüsseleinschränkung teilnehmen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="78a9c-159">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **AssociationSet** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="78a9c-160">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="78a9c-161">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="78a9c-162">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-162">Example</span></span>

<span data-ttu-id="78a9c-163">Das folgende Beispiel zeigt eine **AssociationSet** Element und die repräsentiert die `FK_CustomerOrders` foreign Key-Einschränkung in der zugrunde liegenden Datenbank:</span><span class="sxs-lookup"><span data-stu-id="78a9c-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="78a9c-164">CollectionType-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="78a9c-165">Die **CollectionType** Element in der Datenspeicherschema-Definitionssprache (SSDL) gibt an, dass ein Funktionsrückgabetyp eine Auflistung.</span><span class="sxs-lookup"><span data-stu-id="78a9c-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="78a9c-166">Die **CollectionType** Element ist ein untergeordnetes Element des ReturnType-Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="78a9c-167">Der Typ der Auflistung wird angegeben, mit dem untergeordneten RowType-Element:</span><span class="sxs-lookup"><span data-stu-id="78a9c-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="78a9c-168">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **CollectionType** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="78a9c-169">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="78a9c-170">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="78a9c-171">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-171">Example</span></span>

<span data-ttu-id="78a9c-172">Das folgende Beispiel zeigt eine Funktion, verwendet eine **CollectionType** Element, um anzugeben, dass die Funktion eine Auflistung von Zeilen zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="78a9c-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="78a9c-173">CommandText-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="78a9c-174">Die **CommandText** Element in der Datenspeicherschema-Definitionssprache (SSDL) ist ein untergeordnetes Element des Function-Element, mit dem Sie eine SQL-Anweisung zu definieren, die in der Datenbank ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="78a9c-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="78a9c-175">Die **CommandText** -Element können Sie zum Hinzufügen von Funktionen, die an eine gespeicherte Prozedur in der Datenbank ähnelt, aber Sie definieren die **CommandText** -Element im Speichermodell.</span><span class="sxs-lookup"><span data-stu-id="78a9c-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="78a9c-176">Die **CommandText** Element darf keine untergeordneten Elemente.</span><span class="sxs-lookup"><span data-stu-id="78a9c-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="78a9c-177">Der Hauptteil der **CommandText** Element muss eine gültige SQL­Anweisung für die zugrunde liegende Datenbank sein.</span><span class="sxs-lookup"><span data-stu-id="78a9c-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="78a9c-178">Es sind keine Attribute für die **CommandText** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="78a9c-179">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-179">Example</span></span>

<span data-ttu-id="78a9c-180">Das folgende Beispiel zeigt eine **Funktion** -Element mit einem untergeordneten **CommandText** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="78a9c-181">Bereitstellen der **UpdateProductInOrder** Funktion als Methode für den ObjectContext durch in das konzeptionelle Modell importieren.</span><span class="sxs-lookup"><span data-stu-id="78a9c-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

``` xml
 <Function Name="UpdateProductInOrder" IsComposable="false">
   <CommandText>
     UPDATE Orders
     SET ProductId = @productId
     WHERE OrderId = @orderId;
   </CommandText>
   <Parameter Name="productId"
              Mode="In"
              Type="int"/>
   <Parameter Name="orderId"
              Mode="In"
              Type="int"/>
 </Function>
```

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="78a9c-182">DefiningQuery-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="78a9c-183">Die **DefiningQuery** Element in der Datenspeicherschema-Definitionssprache (SSDL) können Sie eine SQL-Anweisung direkt in der zugrunde liegenden Datenbank ausführen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="78a9c-184">Die **DefiningQuery** Element wird häufig verwendet, wie eine Datenbanksicht, aber die Ansicht im Speichermodell nicht in der Datenbank definiert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="78a9c-185">Die Sicht definiert, die einem **DefiningQuery** Element eines Entitätstyps im konzeptionellen Modell über ein EntitySetMapping-Element zugeordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="78a9c-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="78a9c-186">Diese Zuordnungen sind schreibgeschützt.</span><span class="sxs-lookup"><span data-stu-id="78a9c-186">These mappings are read-only.</span></span>  

<span data-ttu-id="78a9c-187">Die folgende SSDL-Syntax zeigt die Deklaration einer **EntitySet** gefolgt von der **DefiningQuery** Element, das eine Abfrage zum Abrufen der Sicht enthält.</span><span class="sxs-lookup"><span data-stu-id="78a9c-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

``` xml
 <Schema>
     <EntitySet Name="Tables" EntityType="Self.STable">
         <DefiningQuery>
           SELECT  TABLE_CATALOG,
                   'test' as TABLE_SCHEMA,
                   TABLE_NAME
           FROM    INFORMATION_SCHEMA.TABLES
         </DefiningQuery>
     </EntitySet>
 </Schema>
```

<span data-ttu-id="78a9c-188">Sie können gespeicherte Prozeduren zum Aktivieren der Lese-/ schreibszenarien über Ansichten im Entity Framework verwenden.</span><span class="sxs-lookup"><span data-stu-id="78a9c-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span> <span data-ttu-id="78a9c-189">Sie können entweder eine Datenquellensicht oder eine Entity SQL-Sicht als Basistabelle zum Abrufen von Daten und zum Verarbeiten von Änderungen durch gespeicherte Prozeduren verwenden.</span><span class="sxs-lookup"><span data-stu-id="78a9c-189">You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="78a9c-190">Sie können die **DefiningQuery** Element in Microsoft SQL Server Compact 3.5 als Ziel.</span><span class="sxs-lookup"><span data-stu-id="78a9c-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="78a9c-191">Obwohl SQL Server Compact 3.5 keine gespeicherte Prozeduren unterstützt, können Sie ähnliche Funktionalität mit implementieren die **DefiningQuery** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="78a9c-192">Es kann auch beim Erstellen gespeicherter Prozeduren nützlich sein, um einen Konflikt zwischen den in der Programmiersprache und den in der Datenquelle verwendeten Datentypen zu lösen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="78a9c-193">Sie könnten eine **DefiningQuery** , die einen bestimmten Satz von Parametern akzeptiert und ruft dann eine gespeicherte Prozedur mit einem anderen Satz von Parametern, z. B. eine gespeicherte Prozedur, die Daten löscht.</span><span class="sxs-lookup"><span data-stu-id="78a9c-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="78a9c-194">Abhängiges Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="78a9c-195">Die **abhängige** Element in der Datenspeicherschema-Definitionssprache (SSDL) ist ein untergeordnetes Element mit dem ReferentialConstraint-Element, das abhängige Ende einer fremdschlüsseleinschränkung (auch referenzielle Einschränkung genannt) definiert.</span><span class="sxs-lookup"><span data-stu-id="78a9c-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="78a9c-196">Die **abhängige** Element gibt die Spalte (oder Spalten) in einer Tabelle, die eine primäre Schlüsselspalte (oder Spalten) zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="78a9c-197">**PropertyRef** Elemente anzugeben, welche Spalten verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="78a9c-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="78a9c-198">Der Prinzipal Element gibt die Primärschlüsselspalten, die durch Spalten, die im angegebenen verwiesen werden die **abhängige** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="78a9c-199">Die **abhängige** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="78a9c-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="78a9c-200">PropertyRef (mindestens)</span><span class="sxs-lookup"><span data-stu-id="78a9c-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="78a9c-201">Anmerkungselemente (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="78a9c-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="78a9c-202">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="78a9c-202">Applicable Attributes</span></span>

<span data-ttu-id="78a9c-203">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **abhängige** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="78a9c-204">Attributname</span><span class="sxs-lookup"><span data-stu-id="78a9c-204">Attribute Name</span></span> | <span data-ttu-id="78a9c-205">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="78a9c-205">Is Required</span></span> | <span data-ttu-id="78a9c-206">Wert</span><span class="sxs-lookup"><span data-stu-id="78a9c-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="78a9c-207">**Rolle**</span><span class="sxs-lookup"><span data-stu-id="78a9c-207">**Role**</span></span>       | <span data-ttu-id="78a9c-208">Ja</span><span class="sxs-lookup"><span data-stu-id="78a9c-208">Yes</span></span>         | <span data-ttu-id="78a9c-209">Der gleiche Wert wie die **Rolle** Attributs (sofern verwendet) des entsprechenden Elements Ende; andernfalls der Name der Tabelle enthält, die die verweisende Spalte.</span><span class="sxs-lookup"><span data-stu-id="78a9c-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="78a9c-210">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **abhängige** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="78a9c-211">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="78a9c-212">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="78a9c-213">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-213">Example</span></span>

<span data-ttu-id="78a9c-214">Das folgende Beispiel zeigt ein Association-Element, das verwendet eine **ReferentialConstraint** Element, um die Spalten anzugeben, die Teilnahme an der **FK\_CustomerOrders** Fremdschlüssel Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="78a9c-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="78a9c-215">Die **abhängige** -Element gibt an die **"CustomerID"** Spalte die **Reihenfolge** Tabelle als das abhängige Ende der Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="78a9c-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="documentation-element-ssdl"></a><span data-ttu-id="78a9c-216">Documentation-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="78a9c-217">Die **Dokumentation** -Element in der Datenspeicherschema-Definitionssprache (SSDL) verwendet werden kann, um Informationen zu einem Objekt bereitzustellen, die in einem übergeordneten Element definiert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="78a9c-218">Die **Dokumentation** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="78a9c-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="78a9c-219">**Zusammenfassung**: eine kurze Beschreibung des übergeordneten Elements.</span><span class="sxs-lookup"><span data-stu-id="78a9c-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="78a9c-220">(kein (Null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="78a9c-220">(zero or one element)</span></span>
-   <span data-ttu-id="78a9c-221">**LongDescription**: eine ausführliche Beschreibung des übergeordneten Elements.</span><span class="sxs-lookup"><span data-stu-id="78a9c-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="78a9c-222">(kein (Null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="78a9c-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="78a9c-223">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="78a9c-223">Applicable Attributes</span></span>

<span data-ttu-id="78a9c-224">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Dokumentation** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="78a9c-225">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="78a9c-226">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="78a9c-227">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-227">Example</span></span>

<span data-ttu-id="78a9c-228">Das folgende Beispiel zeigt die **Dokumentation** Element als untergeordnetes Element eines EntityType-Elements.</span><span class="sxs-lookup"><span data-stu-id="78a9c-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="end-element-ssdl"></a><span data-ttu-id="78a9c-229">End-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-229">End Element (SSDL)</span></span>

<span data-ttu-id="78a9c-230">Die **End** Element in der Datenspeicherschema-Definitionssprache (SSDL) gibt die Tabelle und die Anzahl der Zeilen an einem Ende einer fremdschlüsseleinschränkung in der zugrunde liegenden Datenbank an.</span><span class="sxs-lookup"><span data-stu-id="78a9c-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="78a9c-231">Die **End** Element kann ein untergeordnetes Element des Association-Element oder das AssociationSet-Element sein.</span><span class="sxs-lookup"><span data-stu-id="78a9c-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="78a9c-232">In jedem dieser Fälle sind andere untergeordnete Elemente und anwendbare Attribute die möglich.</span><span class="sxs-lookup"><span data-stu-id="78a9c-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="78a9c-233">Das End-Element als untergeordnetes Objekt des Association-Elements</span><span class="sxs-lookup"><span data-stu-id="78a9c-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="78a9c-234">Ein **End** Element (als untergeordnetes Element der **Zuordnung** Element) gibt die Tabelle und die Anzahl der Zeilen am Ende einer foreign Key-Einschränkung mit der **Typ** und **Multiplizität** -Attribut an.</span><span class="sxs-lookup"><span data-stu-id="78a9c-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="78a9c-235">Die Enden einer Fremdschlüsseleinschränkung sind als Teil einer SSDL-Zuordnung definiert. Eine SSDL-Zuordnung muss genau zwei Enden aufweisen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="78a9c-236">Ein **End** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="78a9c-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="78a9c-237">Dokumentation (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="78a9c-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="78a9c-238">OnDelete (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="78a9c-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="78a9c-239">Anmerkungselemente (null oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="78a9c-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="78a9c-240">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="78a9c-240">Applicable Attributes</span></span>

<span data-ttu-id="78a9c-241">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **End** -Element, wenn es sich um das untergeordnete Element ist ein **Zuordnung** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="78a9c-242">Attributname</span><span class="sxs-lookup"><span data-stu-id="78a9c-242">Attribute Name</span></span>   | <span data-ttu-id="78a9c-243">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="78a9c-243">Is Required</span></span> | <span data-ttu-id="78a9c-244">Wert</span><span class="sxs-lookup"><span data-stu-id="78a9c-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="78a9c-245">**Type**</span><span class="sxs-lookup"><span data-stu-id="78a9c-245">**Type**</span></span>         | <span data-ttu-id="78a9c-246">Ja</span><span class="sxs-lookup"><span data-stu-id="78a9c-246">Yes</span></span>         | <span data-ttu-id="78a9c-247">Der voll gekennzeichnete Name der SSDL-Entitätenmenge, die am Ende der foreign Key-Einschränkung ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="78a9c-248">**Rolle**</span><span class="sxs-lookup"><span data-stu-id="78a9c-248">**Role**</span></span>         | <span data-ttu-id="78a9c-249">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-249">No</span></span>          | <span data-ttu-id="78a9c-250">Der Wert des der **Rolle** -Attribut im dem Prinzipal- oder abhängigen Element von der entsprechenden ReferentialConstraint-Element (sofern verwendet).</span><span class="sxs-lookup"><span data-stu-id="78a9c-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="78a9c-251">**Multiplizität**</span><span class="sxs-lookup"><span data-stu-id="78a9c-251">**Multiplicity**</span></span> | <span data-ttu-id="78a9c-252">Ja</span><span class="sxs-lookup"><span data-stu-id="78a9c-252">Yes</span></span>         | <span data-ttu-id="78a9c-253">**1**, **0.. 1**, oder **\*** abhängig von der Anzahl der Zeilen, die am Ende der foreign Key-Einschränkung sein können.</span><span class="sxs-lookup"><span data-stu-id="78a9c-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="78a9c-254">**1** gibt an, dass genau eine Zeile, die am Ende foreign Key-Einschränkung vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="78a9c-255">**0.. 1** gibt an, dass maximal eine Zeile, die am Ende foreign Key-Einschränkung vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="78a9c-256">**\*** Gibt an, dass 0 (null), einer oder mehreren Zeilen am Ende foreign Key-Einschränkung vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="78a9c-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="78a9c-257">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **End** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="78a9c-258">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="78a9c-259">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="78a9c-260">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-260">Example</span></span>

<span data-ttu-id="78a9c-261">Das folgende Beispiel zeigt eine **Zuordnung** -Element, definiert der **FK\_CustomerOrders** foreign Key-Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="78a9c-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="78a9c-262">Die **Multiplizität** auf jedem angegebenen Werte **End** Element anzugeben, dass viele Zeilen in der **Bestellungen** Tabelle kann mit einer Zeile im verbunden werden die **Kunden**  Tabelle, jedoch nur eine Zeile in der **Kunden** Tabelle möglich mit einer Zeile in der **Bestellungen** Tabelle.</span><span class="sxs-lookup"><span data-stu-id="78a9c-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="78a9c-263">Darüber hinaus die **OnDelete** Element gibt an, dass alle Zeilen in der **Bestellungen** Tabelle, die eine bestimmte Zeile in verweisen auf die **Kunden** Tabelle gelöscht werden, wenn die Zeile in die **Kunden** Tabelle gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="78a9c-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="78a9c-264">Das End-Element als untergeordnetes Objekt des AssociationSet-Elements</span><span class="sxs-lookup"><span data-stu-id="78a9c-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="78a9c-265">Die **End** -Element (als untergeordnetes Element der **AssociationSet** Element) gibt eine Tabelle an einem Ende einer fremdschlüsseleinschränkung in der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="78a9c-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="78a9c-266">Ein **End** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="78a9c-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="78a9c-267">Dokumentation (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="78a9c-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="78a9c-268">Anmerkungselemente (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="78a9c-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="78a9c-269">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="78a9c-269">Applicable Attributes</span></span>

<span data-ttu-id="78a9c-270">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **End** -Element, wenn es sich um das untergeordnete Element ist ein **AssociationSet** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="78a9c-271">Attributname</span><span class="sxs-lookup"><span data-stu-id="78a9c-271">Attribute Name</span></span> | <span data-ttu-id="78a9c-272">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="78a9c-272">Is Required</span></span> | <span data-ttu-id="78a9c-273">Wert</span><span class="sxs-lookup"><span data-stu-id="78a9c-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="78a9c-274">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="78a9c-274">**EntitySet**</span></span>  | <span data-ttu-id="78a9c-275">Ja</span><span class="sxs-lookup"><span data-stu-id="78a9c-275">Yes</span></span>         | <span data-ttu-id="78a9c-276">Der Name der SSDL-Entitätenmenge, die am Ende der foreign Key-Einschränkung ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="78a9c-277">**Rolle**</span><span class="sxs-lookup"><span data-stu-id="78a9c-277">**Role**</span></span>       | <span data-ttu-id="78a9c-278">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-278">No</span></span>          | <span data-ttu-id="78a9c-279">Der Wert eines der **Rolle** auf einem angegebenen Attribute **End** Element des entsprechenden Association-Elements.</span><span class="sxs-lookup"><span data-stu-id="78a9c-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="78a9c-280">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **End** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="78a9c-281">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="78a9c-282">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="78a9c-283">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-283">Example</span></span>

<span data-ttu-id="78a9c-284">Das folgende Beispiel zeigt eine **EntityContainer** -Element mit einem **AssociationSet** -Element mit zwei **End** Elemente:</span><span class="sxs-lookup"><span data-stu-id="78a9c-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="78a9c-285">EntityContainer-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="78a9c-286">Ein **EntityContainer** Element in der Datenspeicherschema-Definitionssprache (SSDL) beschreibt die Struktur der zugrunde liegenden Datenquelle in einer Entity Framework-Anwendung: SSDL-Entitätenmengen (definiert in EntitySet-Elemente) stellen Tabellen dar. eine Datenbank, SSDL-Entitätstypen (definiert im EntityType-Elemente) Zeilen in einer Tabelle und Zuordnungssätze (definiert in AssociationSet-Elementen) darstellen foreign Key-Einschränkungen in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="78a9c-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="78a9c-287">Ein Speichermodell-Entitätscontainer wird über die EntityContainerMapping-Element ein konzeptioneller modellentitätscontainer zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="78a9c-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="78a9c-288">Ein **EntityContainer** -Element kann NULL oder eine dokumentationselemente aufweisen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="78a9c-289">Wenn eine **Dokumentation** -Element vorhanden ist, sie müssen vor allen anderen untergeordneten Elementen stehen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="78a9c-290">Ein **EntityContainer** -Element kann 0 (null) oder mehrere der folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet) aufweisen:</span><span class="sxs-lookup"><span data-stu-id="78a9c-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="78a9c-291">EntitySet</span><span class="sxs-lookup"><span data-stu-id="78a9c-291">EntitySet</span></span>
-   <span data-ttu-id="78a9c-292">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="78a9c-292">AssociationSet</span></span>
-   <span data-ttu-id="78a9c-293">Anmerkungselemente</span><span class="sxs-lookup"><span data-stu-id="78a9c-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="78a9c-294">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="78a9c-294">Applicable Attributes</span></span>

<span data-ttu-id="78a9c-295">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **EntityContainer** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="78a9c-296">Attributname</span><span class="sxs-lookup"><span data-stu-id="78a9c-296">Attribute Name</span></span> | <span data-ttu-id="78a9c-297">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="78a9c-297">Is Required</span></span> | <span data-ttu-id="78a9c-298">Wert</span><span class="sxs-lookup"><span data-stu-id="78a9c-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="78a9c-299">**Name**</span><span class="sxs-lookup"><span data-stu-id="78a9c-299">**Name**</span></span>       | <span data-ttu-id="78a9c-300">Ja</span><span class="sxs-lookup"><span data-stu-id="78a9c-300">Yes</span></span>         | <span data-ttu-id="78a9c-301">Der Name des Entitätscontainers.</span><span class="sxs-lookup"><span data-stu-id="78a9c-301">The name of the entity container.</span></span> <span data-ttu-id="78a9c-302">Dieser Name darf keine Punkte (.) enthalten.</span><span class="sxs-lookup"><span data-stu-id="78a9c-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="78a9c-303">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **EntityContainer** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="78a9c-304">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="78a9c-305">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="78a9c-306">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-306">Example</span></span>

<span data-ttu-id="78a9c-307">Das folgende Beispiel zeigt eine **EntityContainer** Element, das zwei Entitätenmengen und einen Zuordnungssatz definiert.</span><span class="sxs-lookup"><span data-stu-id="78a9c-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="78a9c-308">Beachten Sie, dass die Namen des Entitätstyps und des Zuordnungstyps mit dem Namespace des konzeptionellen Modellnamens qualifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="78a9c-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entityset-element-ssdl"></a><span data-ttu-id="78a9c-309">EntitySet-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="78a9c-310">Ein **EntitySet** -Element der Datenspeicherschema-Definitionssprache (SSDL) stellt dar, eine Tabelle oder Sicht in der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="78a9c-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="78a9c-311">Ein EntityType-Element in SSDL stellt eine Zeile in der Tabelle oder Sicht dar.</span><span class="sxs-lookup"><span data-stu-id="78a9c-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="78a9c-312">Die **EntityType** Attribut eine **EntitySet** Element gibt an, die bestimmten SSDL-Entitätstyp, der Zeilen in einer SSDL-Entitätenmenge darstellt.</span><span class="sxs-lookup"><span data-stu-id="78a9c-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="78a9c-313">Die Zuordnung zwischen einer CSDL-Entitätenmenge und einer SSDL-Entitätenmenge, die in einem EntitySetMapping-Element angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="78a9c-314">Die **EntitySet** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="78a9c-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="78a9c-315">Dokumentation (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="78a9c-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="78a9c-316">DefiningQuery (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="78a9c-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="78a9c-317">Anmerkungselemente</span><span class="sxs-lookup"><span data-stu-id="78a9c-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="78a9c-318">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="78a9c-318">Applicable Attributes</span></span>

<span data-ttu-id="78a9c-319">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **EntitySet** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="78a9c-320">Einige Attribute (hier nicht aufgeführt) qualifiziert werden können, mit der **speichern** Alias.</span><span class="sxs-lookup"><span data-stu-id="78a9c-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="78a9c-321">Diese Attribute werden durch den Modellaktualisierungs-Assistenten verwendet, wenn ein Modell aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="78a9c-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="78a9c-322">Attributname</span><span class="sxs-lookup"><span data-stu-id="78a9c-322">Attribute Name</span></span> | <span data-ttu-id="78a9c-323">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="78a9c-323">Is Required</span></span> | <span data-ttu-id="78a9c-324">Wert</span><span class="sxs-lookup"><span data-stu-id="78a9c-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="78a9c-325">**Name**</span><span class="sxs-lookup"><span data-stu-id="78a9c-325">**Name**</span></span>       | <span data-ttu-id="78a9c-326">Ja</span><span class="sxs-lookup"><span data-stu-id="78a9c-326">Yes</span></span>         | <span data-ttu-id="78a9c-327">Der Name des Entitätssatzes.</span><span class="sxs-lookup"><span data-stu-id="78a9c-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="78a9c-328">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="78a9c-328">**EntityType**</span></span> | <span data-ttu-id="78a9c-329">Ja</span><span class="sxs-lookup"><span data-stu-id="78a9c-329">Yes</span></span>         | <span data-ttu-id="78a9c-330">Der vollqualifizierte Name des Entitätstyps, für den der Entitätssatz Instanzen enthält.</span><span class="sxs-lookup"><span data-stu-id="78a9c-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="78a9c-331">**Schema**</span><span class="sxs-lookup"><span data-stu-id="78a9c-331">**Schema**</span></span>     | <span data-ttu-id="78a9c-332">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-332">No</span></span>          | <span data-ttu-id="78a9c-333">Das Datenbankschema.</span><span class="sxs-lookup"><span data-stu-id="78a9c-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="78a9c-334">**Table**</span><span class="sxs-lookup"><span data-stu-id="78a9c-334">**Table**</span></span>      | <span data-ttu-id="78a9c-335">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-335">No</span></span>          | <span data-ttu-id="78a9c-336">Die Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="78a9c-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="78a9c-337">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **EntitySet** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="78a9c-338">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="78a9c-339">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="78a9c-340">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-340">Example</span></span>

<span data-ttu-id="78a9c-341">Das folgende Beispiel zeigt eine **EntityContainer** -Element, das zwei **EntitySet** Elemente und ein **AssociationSet** Element:</span><span class="sxs-lookup"><span data-stu-id="78a9c-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

``` xml
 <EntityContainer Name="ExampleModelStoreContainer">
   <EntitySet Name="Customers"
              EntityType="ExampleModel.Store.Customers"
              Schema="dbo" />
   <EntitySet Name="Orders"
              EntityType="ExampleModel.Store.Orders"
              Schema="dbo" />
   <AssociationSet Name="FK_CustomerOrders"
                   Association="ExampleModel.Store.FK_CustomerOrders">
     <End Role="Customers" EntitySet="Customers" />
     <End Role="Orders" EntitySet="Orders" />
   </AssociationSet>
 </EntityContainer>
```

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="78a9c-342">EntityType-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="78a9c-343">Ein **EntityType** Element in der Datenspeicherschema-Definitionssprache (SSDL) stellt eine Zeile in einer Tabelle oder Sicht der zugrunde liegenden Datenbank dar.</span><span class="sxs-lookup"><span data-stu-id="78a9c-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="78a9c-344">Ein EntitySet-Element in SSDL stellt dar, die Tabelle oder Sicht, die in der Zeilen enthalten.</span><span class="sxs-lookup"><span data-stu-id="78a9c-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="78a9c-345">Die **EntityType** Attribut eine **EntitySet** Element gibt an, die bestimmten SSDL-Entitätstyp, der Zeilen in einer SSDL-Entitätenmenge darstellt.</span><span class="sxs-lookup"><span data-stu-id="78a9c-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="78a9c-346">Die Zuordnung zwischen einer SSDL-Entitätstyp und einer CSDL-Entitätstyp wird in einem EntityTypeMapping-Element angegeben.</span><span class="sxs-lookup"><span data-stu-id="78a9c-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="78a9c-347">Die **EntityType** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="78a9c-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="78a9c-348">Dokumentation (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="78a9c-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="78a9c-349">Schlüssel (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="78a9c-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="78a9c-350">Anmerkungselemente</span><span class="sxs-lookup"><span data-stu-id="78a9c-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="78a9c-351">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="78a9c-351">Applicable Attributes</span></span>

<span data-ttu-id="78a9c-352">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **EntityType** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="78a9c-353">Attributname</span><span class="sxs-lookup"><span data-stu-id="78a9c-353">Attribute Name</span></span> | <span data-ttu-id="78a9c-354">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="78a9c-354">Is Required</span></span> | <span data-ttu-id="78a9c-355">Wert</span><span class="sxs-lookup"><span data-stu-id="78a9c-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="78a9c-356">**Name**</span><span class="sxs-lookup"><span data-stu-id="78a9c-356">**Name**</span></span>       | <span data-ttu-id="78a9c-357">Ja</span><span class="sxs-lookup"><span data-stu-id="78a9c-357">Yes</span></span>         | <span data-ttu-id="78a9c-358">Der Name des Entitätstyps.</span><span class="sxs-lookup"><span data-stu-id="78a9c-358">The name of the entity type.</span></span> <span data-ttu-id="78a9c-359">Dieser Wert ist normalerweise der gleiche wie der Name der Tabelle, in der der Entitätstyp eine Zeile darstellt.</span><span class="sxs-lookup"><span data-stu-id="78a9c-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="78a9c-360">Dieser Wert darf keine Punkte (.) enthalten.</span><span class="sxs-lookup"><span data-stu-id="78a9c-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="78a9c-361">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **EntityType** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="78a9c-362">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="78a9c-363">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="78a9c-364">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-364">Example</span></span>

<span data-ttu-id="78a9c-365">Das folgende Beispiel zeigt eine **EntityType** -Element mit zwei Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="78a9c-365">The following example shows an **EntityType** element with two properties:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="function-element-ssdl"></a><span data-ttu-id="78a9c-366">Function-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-366">Function Element (SSDL)</span></span>

<span data-ttu-id="78a9c-367">Die **Funktion** Element in der Datenspeicherschema-Definitionssprache (SSDL) gibt eine vorhandene gespeicherte Prozedur in der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="78a9c-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="78a9c-368">Die **Funktion** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="78a9c-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="78a9c-369">Dokumentation (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="78a9c-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="78a9c-370">Parameter (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="78a9c-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="78a9c-371">CommandText-Eigenschaft (0 (null) oder 1)</span><span class="sxs-lookup"><span data-stu-id="78a9c-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="78a9c-372">ReturnType (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="78a9c-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="78a9c-373">Anmerkungselemente (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="78a9c-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="78a9c-374">Ein Rückgabewert für eine Funktion muss angegeben werden, entweder mit der **ReturnType** Element oder die **ReturnType** -Attribut (siehe unten), aber nicht beides.</span><span class="sxs-lookup"><span data-stu-id="78a9c-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="78a9c-375">Gespeicherte Prozeduren, die im Speichermodell angegeben sind, können in das konzeptionelle Modell einer Anwendung importiert werden.</span><span class="sxs-lookup"><span data-stu-id="78a9c-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="78a9c-376">Weitere Informationen finden Sie unter [Abfragen mit gespeicherten Prozeduren](~/ef6/modeling/designer/stored-procedures/query.md).</span><span class="sxs-lookup"><span data-stu-id="78a9c-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="78a9c-377">Die **Funktion** -Element kann auch verwendet werden, um benutzerdefinierte Funktionen im Speichermodell zu definieren.</span><span class="sxs-lookup"><span data-stu-id="78a9c-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="78a9c-378">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="78a9c-378">Applicable Attributes</span></span>

<span data-ttu-id="78a9c-379">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Funktion** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="78a9c-380">Einige Attribute (hier nicht aufgeführt) qualifiziert werden können, mit der **speichern** Alias.</span><span class="sxs-lookup"><span data-stu-id="78a9c-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="78a9c-381">Diese Attribute werden durch den Modellaktualisierungs-Assistenten verwendet, wenn ein Modell aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="78a9c-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="78a9c-382">Attributname</span><span class="sxs-lookup"><span data-stu-id="78a9c-382">Attribute Name</span></span>             | <span data-ttu-id="78a9c-383">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="78a9c-383">Is Required</span></span> | <span data-ttu-id="78a9c-384">Wert</span><span class="sxs-lookup"><span data-stu-id="78a9c-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="78a9c-385">**Name**</span><span class="sxs-lookup"><span data-stu-id="78a9c-385">**Name**</span></span>                   | <span data-ttu-id="78a9c-386">Ja</span><span class="sxs-lookup"><span data-stu-id="78a9c-386">Yes</span></span>         | <span data-ttu-id="78a9c-387">Der Name der gespeicherten Prozedur.</span><span class="sxs-lookup"><span data-stu-id="78a9c-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="78a9c-388">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="78a9c-388">**ReturnType**</span></span>             | <span data-ttu-id="78a9c-389">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-389">No</span></span>          | <span data-ttu-id="78a9c-390">Der Rückgabetyp der gespeicherten Prozedur.</span><span class="sxs-lookup"><span data-stu-id="78a9c-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="78a9c-391">**Aggregat**</span><span class="sxs-lookup"><span data-stu-id="78a9c-391">**Aggregate**</span></span>              | <span data-ttu-id="78a9c-392">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-392">No</span></span>          | <span data-ttu-id="78a9c-393">**"True"** , wenn die gespeicherte Prozedur einen Aggregatwert zurückgibt. andernfalls **"false"**.</span><span class="sxs-lookup"><span data-stu-id="78a9c-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="78a9c-394">**"Vordefiniert"**</span><span class="sxs-lookup"><span data-stu-id="78a9c-394">**BuiltIn**</span></span>                | <span data-ttu-id="78a9c-395">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-395">No</span></span>          | <span data-ttu-id="78a9c-396">**"True"** ist die Funktion eine integrierte<sup>1</sup> funktioniert; andernfalls **"false"**.</span><span class="sxs-lookup"><span data-stu-id="78a9c-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="78a9c-397">**StoreFunctionName**</span><span class="sxs-lookup"><span data-stu-id="78a9c-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="78a9c-398">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-398">No</span></span>          | <span data-ttu-id="78a9c-399">Der Name der gespeicherten Prozedur.</span><span class="sxs-lookup"><span data-stu-id="78a9c-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="78a9c-400">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="78a9c-400">**NiladicFunction**</span></span>        | <span data-ttu-id="78a9c-401">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-401">No</span></span>          | <span data-ttu-id="78a9c-402">**"True"** ist die Funktion eine Niladic<sup>2</sup> Funktion. **"False"** andernfalls.</span><span class="sxs-lookup"><span data-stu-id="78a9c-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="78a9c-403">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="78a9c-403">**IsComposable**</span></span>           | <span data-ttu-id="78a9c-404">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-404">No</span></span>          | <span data-ttu-id="78a9c-405">**"True"** ist die Funktion eine zusammensetzbare<sup>3</sup> Funktion. **"False"** andernfalls.</span><span class="sxs-lookup"><span data-stu-id="78a9c-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="78a9c-406">**ParameterTypeSemantics**</span><span class="sxs-lookup"><span data-stu-id="78a9c-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="78a9c-407">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-407">No</span></span>          | <span data-ttu-id="78a9c-408">Die Enumeration, die die Typsemantik definiert, die zum Auflösen von Funktionsüberladungen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="78a9c-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="78a9c-409">Die Enumeration ist im Anbietermanifest für jede Funktionsdefinition definiert.</span><span class="sxs-lookup"><span data-stu-id="78a9c-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="78a9c-410">Der Standardwert ist **AllowImplicitConversion**.</span><span class="sxs-lookup"><span data-stu-id="78a9c-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="78a9c-411">**Schema**</span><span class="sxs-lookup"><span data-stu-id="78a9c-411">**Schema**</span></span>                 | <span data-ttu-id="78a9c-412">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-412">No</span></span>          | <span data-ttu-id="78a9c-413">Der Name des Schemas, in dem die gespeicherte Prozedur definiert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="78a9c-414"><sup>1</sup> eine integrierte Funktion ist eine Funktion, die in der Datenbank definiert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="78a9c-415">Informationen zu Funktionen, die im Speichermodell definiert sind, finden Sie unter CommandText-Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="78a9c-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="78a9c-416"><sup>2</sup> eine Niladic-Funktion ist eine Funktion, die keine Parameter akzeptiert und bei Aufruf keine Klammern erforderlich.</span><span class="sxs-lookup"><span data-stu-id="78a9c-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="78a9c-417"><sup>3</sup> zwei Funktionen sind zusammensetzbar, wenn die Ausgabe einer Funktion die Eingabe für die andere Funktion werden kann.</span><span class="sxs-lookup"><span data-stu-id="78a9c-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="78a9c-418">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Funktion** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="78a9c-419">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="78a9c-420">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="78a9c-421">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-421">Example</span></span>

<span data-ttu-id="78a9c-422">Das folgende Beispiel zeigt eine **Funktion** Element, das entspricht, dem **UpdateOrderQuantity** gespeicherte Prozedur.</span><span class="sxs-lookup"><span data-stu-id="78a9c-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="78a9c-423">Die gespeicherte Prozedur akzeptiert zwei Parameter und gibt keinen Wert zurück.</span><span class="sxs-lookup"><span data-stu-id="78a9c-423">The stored procedure accepts two parameters and does not return a value.</span></span>

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="key-element-ssdl"></a><span data-ttu-id="78a9c-424">Key-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-424">Key Element (SSDL)</span></span>

<span data-ttu-id="78a9c-425">Die **Schlüssel** -Element der Datenspeicherschema-Definitionssprache (SSDL) stellt den Primärschlüssel einer Tabelle in der zugrunde liegenden Datenbank dar.</span><span class="sxs-lookup"><span data-stu-id="78a9c-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="78a9c-426">**Schlüssel** ist ein untergeordnetes Element des EntityType-Element, das eine Zeile in einer Tabelle darstellt.</span><span class="sxs-lookup"><span data-stu-id="78a9c-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="78a9c-427">Der Primärschlüssel wird definiert, der **Schlüssel** Element durch einen Verweis auf ein oder mehrere Eigenschaft-Elemente, die auf definiert sind die **EntityType** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="78a9c-428">Die **Schlüssel** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="78a9c-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="78a9c-429">PropertyRef (mindestens)</span><span class="sxs-lookup"><span data-stu-id="78a9c-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="78a9c-430">Anmerkungselemente</span><span class="sxs-lookup"><span data-stu-id="78a9c-430">Annotation elements</span></span>

<span data-ttu-id="78a9c-431">Es sind keine Attribute für die **Schlüssel** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="78a9c-432">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-432">Example</span></span>

<span data-ttu-id="78a9c-433">Das folgende Beispiel zeigt eine **EntityType** Elements mit einem Schlüssel, der eine Eigenschaft verweist:</span><span class="sxs-lookup"><span data-stu-id="78a9c-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="78a9c-434">OnDelete-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="78a9c-435">Die **OnDelete** Element in der Datenspeicherschema-Definitionssprache (SSDL) spiegelt das Datenbankverhalten wider, wenn eine Zeile, die Teil einer foreign Key-Einschränkung gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="78a9c-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="78a9c-436">Wenn die Aktion, um festgelegt ist **Cascade**, und klicken Sie dann die Zeilen, die eine Zeile verweisen, die gelöscht wird ebenfalls gelöscht werden.</span><span class="sxs-lookup"><span data-stu-id="78a9c-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="78a9c-437">Wenn die Aktion, um festgelegt ist **keine**, und klicken Sie dann die Zeilen, die eine Zeile verweisen, die gelöscht wird, nicht gelöscht.</span><span class="sxs-lookup"><span data-stu-id="78a9c-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="78a9c-438">Ein **OnDelete** Element ist ein untergeordnetes Element eines End-Elements.</span><span class="sxs-lookup"><span data-stu-id="78a9c-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="78a9c-439">Ein **OnDelete** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="78a9c-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="78a9c-440">Dokumentation (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="78a9c-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="78a9c-441">Anmerkungselemente (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="78a9c-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="78a9c-442">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="78a9c-442">Applicable Attributes</span></span>

<span data-ttu-id="78a9c-443">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **OnDelete** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="78a9c-444">Attributname</span><span class="sxs-lookup"><span data-stu-id="78a9c-444">Attribute Name</span></span> | <span data-ttu-id="78a9c-445">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="78a9c-445">Is Required</span></span> | <span data-ttu-id="78a9c-446">Wert</span><span class="sxs-lookup"><span data-stu-id="78a9c-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="78a9c-447">**Aktion**</span><span class="sxs-lookup"><span data-stu-id="78a9c-447">**Action**</span></span>     | <span data-ttu-id="78a9c-448">Ja</span><span class="sxs-lookup"><span data-stu-id="78a9c-448">Yes</span></span>         | <span data-ttu-id="78a9c-449">**CASCADE** oder **keine**.</span><span class="sxs-lookup"><span data-stu-id="78a9c-449">**Cascade** or **None**.</span></span> <span data-ttu-id="78a9c-450">(Der Wert **Restricted** ist gültig, aber hat das gleiche Verhalten wie **keine**.)</span><span class="sxs-lookup"><span data-stu-id="78a9c-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="78a9c-451">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **OnDelete** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="78a9c-452">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="78a9c-453">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="78a9c-454">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-454">Example</span></span>

<span data-ttu-id="78a9c-455">Das folgende Beispiel zeigt eine **Zuordnung** -Element, definiert der **FK\_CustomerOrders** foreign Key-Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="78a9c-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="78a9c-456">Die **OnDelete** Element gibt an, dass alle Zeilen in der **Bestellungen** Tabelle, die eine bestimmte Zeile in verweisen auf die **Kunden** Tabelle gelöscht werden, wenn die Zeile in der **Kunden** Tabelle gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="78a9c-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="parameter-element-ssdl"></a><span data-ttu-id="78a9c-457">Parameter-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="78a9c-458">Die **Parameter** Element in der Datenspeicherschema-Definitionssprache (SSDL) ist ein untergeordnetes Element des Function-Element, das Parameter für eine gespeicherte Prozedur in der Datenbank angibt.</span><span class="sxs-lookup"><span data-stu-id="78a9c-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="78a9c-459">Die **Parameter** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="78a9c-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="78a9c-460">Dokumentation (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="78a9c-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="78a9c-461">Anmerkungselemente (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="78a9c-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="78a9c-462">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="78a9c-462">Applicable Attributes</span></span>

<span data-ttu-id="78a9c-463">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Parameter** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="78a9c-464">Attributname</span><span class="sxs-lookup"><span data-stu-id="78a9c-464">Attribute Name</span></span> | <span data-ttu-id="78a9c-465">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="78a9c-465">Is Required</span></span> | <span data-ttu-id="78a9c-466">Wert</span><span class="sxs-lookup"><span data-stu-id="78a9c-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="78a9c-467">**Name**</span><span class="sxs-lookup"><span data-stu-id="78a9c-467">**Name**</span></span>       | <span data-ttu-id="78a9c-468">Ja</span><span class="sxs-lookup"><span data-stu-id="78a9c-468">Yes</span></span>         | <span data-ttu-id="78a9c-469">Der Name des Parameters.</span><span class="sxs-lookup"><span data-stu-id="78a9c-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="78a9c-470">**Type**</span><span class="sxs-lookup"><span data-stu-id="78a9c-470">**Type**</span></span>       | <span data-ttu-id="78a9c-471">Ja</span><span class="sxs-lookup"><span data-stu-id="78a9c-471">Yes</span></span>         | <span data-ttu-id="78a9c-472">Der Typ des Parameters.</span><span class="sxs-lookup"><span data-stu-id="78a9c-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="78a9c-473">**Modus**</span><span class="sxs-lookup"><span data-stu-id="78a9c-473">**Mode**</span></span>       | <span data-ttu-id="78a9c-474">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-474">No</span></span>          | <span data-ttu-id="78a9c-475">**In**, **Out**, oder **InOut** abhängig davon, ob der Parameter eine Eingabe, Ausgabe oder Eingabe-/Ausgabeparameter.</span><span class="sxs-lookup"><span data-stu-id="78a9c-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="78a9c-476">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="78a9c-476">**MaxLength**</span></span>  | <span data-ttu-id="78a9c-477">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-477">No</span></span>          | <span data-ttu-id="78a9c-478">Die maximale Länge des Parameters.</span><span class="sxs-lookup"><span data-stu-id="78a9c-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="78a9c-479">**Genauigkeit**</span><span class="sxs-lookup"><span data-stu-id="78a9c-479">**Precision**</span></span>  | <span data-ttu-id="78a9c-480">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-480">No</span></span>          | <span data-ttu-id="78a9c-481">Die Genauigkeit des Parameters.</span><span class="sxs-lookup"><span data-stu-id="78a9c-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="78a9c-482">**Scale** (Skalieren)</span><span class="sxs-lookup"><span data-stu-id="78a9c-482">**Scale**</span></span>      | <span data-ttu-id="78a9c-483">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-483">No</span></span>          | <span data-ttu-id="78a9c-484">Die Skalierung des Parameters.</span><span class="sxs-lookup"><span data-stu-id="78a9c-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="78a9c-485">**SRID**</span><span class="sxs-lookup"><span data-stu-id="78a9c-485">**SRID**</span></span>       | <span data-ttu-id="78a9c-486">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-486">No</span></span>          | <span data-ttu-id="78a9c-487">Räumliche Verweis Systembezeichner.</span><span class="sxs-lookup"><span data-stu-id="78a9c-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="78a9c-488">Gilt nur für Parameter des räumlichen Typen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="78a9c-489">Weitere Informationen finden Sie unter [SRID](http://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="78a9c-489">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="78a9c-490">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Parameter** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="78a9c-491">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="78a9c-492">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="78a9c-493">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-493">Example</span></span>

<span data-ttu-id="78a9c-494">Das folgende Beispiel zeigt eine **Funktion** -Element, das zwei **Parameter** Elemente, die Eingabeparameter angeben:</span><span class="sxs-lookup"><span data-stu-id="78a9c-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

``` xml
 <Function Name="UpdateOrderQuantity"
           Aggregate="false"
           BuiltIn="false"
           NiladicFunction="false"
           IsComposable="false"
           ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="orderId" Type="int" Mode="In" />
   <Parameter Name="newQuantity" Type="int" Mode="In" />
 </Function>
```

## <a name="principal-element-ssdl"></a><span data-ttu-id="78a9c-495">Prinzipalelement (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="78a9c-496">Die **Principal** Element in der Datenspeicherschema-Definitionssprache (SSDL) ist ein untergeordnetes Element mit dem ReferentialConstraint-Element, das prinzipalende einer fremdschlüsseleinschränkung (auch referenzielle Einschränkung genannt) definiert.</span><span class="sxs-lookup"><span data-stu-id="78a9c-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="78a9c-497">Die **Principal** Element gibt die Primärschlüsselspalte (oder Spalten) in einer Tabelle, die von einer anderen Spalte (oder Spalten) verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="78a9c-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="78a9c-498">**PropertyRef** Elemente anzugeben, welche Spalten verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="78a9c-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="78a9c-499">Dependent-Element gibt die Spalten, die die Primärschlüsselspalten, die im angegebenen verweisen die **Principal** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="78a9c-500">Die **Principal** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="78a9c-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="78a9c-501">PropertyRef (mindestens)</span><span class="sxs-lookup"><span data-stu-id="78a9c-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="78a9c-502">Anmerkungselemente (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="78a9c-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="78a9c-503">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="78a9c-503">Applicable Attributes</span></span>

<span data-ttu-id="78a9c-504">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Principal** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="78a9c-505">Attributname</span><span class="sxs-lookup"><span data-stu-id="78a9c-505">Attribute Name</span></span> | <span data-ttu-id="78a9c-506">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="78a9c-506">Is Required</span></span> | <span data-ttu-id="78a9c-507">Wert</span><span class="sxs-lookup"><span data-stu-id="78a9c-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="78a9c-508">**Rolle**</span><span class="sxs-lookup"><span data-stu-id="78a9c-508">**Role**</span></span>       | <span data-ttu-id="78a9c-509">Ja</span><span class="sxs-lookup"><span data-stu-id="78a9c-509">Yes</span></span>         | <span data-ttu-id="78a9c-510">Der gleiche Wert wie die **Rolle** Attributs (sofern verwendet) des entsprechenden Elements Ende; andernfalls der Name der Tabelle enthält die Spalte, auf die verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="78a9c-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="78a9c-511">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Principal** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="78a9c-512">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="78a9c-513">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="78a9c-514">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-514">Example</span></span>

<span data-ttu-id="78a9c-515">Das folgende Beispiel zeigt ein Association-Element, das verwendet eine **ReferentialConstraint** Element, um die Spalten anzugeben, die Teilnahme an der **FK\_CustomerOrders** Fremdschlüssel Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="78a9c-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="78a9c-516">Die **Principal** -Element gibt an die **"CustomerID"** Spalte die **Kunden** Tabelle als das prinzipalende der Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="78a9c-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="property-element-ssdl"></a><span data-ttu-id="78a9c-517">Property-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-517">Property Element (SSDL)</span></span>

<span data-ttu-id="78a9c-518">Die **Eigenschaft** Element in der Datenspeicherschema-Definitionssprache (SSDL) stellt eine Spalte in einer Tabelle in der zugrunde liegenden Datenbank dar.</span><span class="sxs-lookup"><span data-stu-id="78a9c-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="78a9c-519">**Eigenschaft** Elemente sind untergeordnete Elemente des EntityType-Elemente, die Zeilen in einer Tabelle darstellen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="78a9c-520">Jede **Eigenschaft** Element definiert eine **EntityType** -Element stellt eine Spalte dar.</span><span class="sxs-lookup"><span data-stu-id="78a9c-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="78a9c-521">Ein **Eigenschaft** Element kann keine untergeordneten Elemente aufweisen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="78a9c-522">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="78a9c-522">Applicable Attributes</span></span>

<span data-ttu-id="78a9c-523">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Eigenschaft** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="78a9c-524">Attributname</span><span class="sxs-lookup"><span data-stu-id="78a9c-524">Attribute Name</span></span>            | <span data-ttu-id="78a9c-525">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="78a9c-525">Is Required</span></span> | <span data-ttu-id="78a9c-526">Wert</span><span class="sxs-lookup"><span data-stu-id="78a9c-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="78a9c-527">**Name**</span><span class="sxs-lookup"><span data-stu-id="78a9c-527">**Name**</span></span>                  | <span data-ttu-id="78a9c-528">Ja</span><span class="sxs-lookup"><span data-stu-id="78a9c-528">Yes</span></span>         | <span data-ttu-id="78a9c-529">Der Name der zugehörigen Spalte.</span><span class="sxs-lookup"><span data-stu-id="78a9c-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="78a9c-530">**Type**</span><span class="sxs-lookup"><span data-stu-id="78a9c-530">**Type**</span></span>                  | <span data-ttu-id="78a9c-531">Ja</span><span class="sxs-lookup"><span data-stu-id="78a9c-531">Yes</span></span>         | <span data-ttu-id="78a9c-532">Der Typ der zugehörigen Spalte.</span><span class="sxs-lookup"><span data-stu-id="78a9c-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="78a9c-533">**NULL-Werte zulässt**</span><span class="sxs-lookup"><span data-stu-id="78a9c-533">**Nullable**</span></span>              | <span data-ttu-id="78a9c-534">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-534">No</span></span>          | <span data-ttu-id="78a9c-535">**"True"** (Standardwert) oder **"false"** abhängig davon, ob die entsprechende Spalte einen null-Wert haben kann.</span><span class="sxs-lookup"><span data-stu-id="78a9c-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="78a9c-536">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="78a9c-536">**DefaultValue**</span></span>          | <span data-ttu-id="78a9c-537">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-537">No</span></span>          | <span data-ttu-id="78a9c-538">Der Standardwert der zugehörigen Spalte.</span><span class="sxs-lookup"><span data-stu-id="78a9c-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="78a9c-539">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="78a9c-539">**MaxLength**</span></span>             | <span data-ttu-id="78a9c-540">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-540">No</span></span>          | <span data-ttu-id="78a9c-541">Maximale Länge der zugehörigen Spalte.</span><span class="sxs-lookup"><span data-stu-id="78a9c-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="78a9c-542">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="78a9c-542">**FixedLength**</span></span>           | <span data-ttu-id="78a9c-543">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-543">No</span></span>          | <span data-ttu-id="78a9c-544">**"True"** oder **"false"** abhängig davon, ob der entsprechende Spaltenwert als Zeichenfolge mit fester Länge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="78a9c-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="78a9c-545">**Genauigkeit**</span><span class="sxs-lookup"><span data-stu-id="78a9c-545">**Precision**</span></span>             | <span data-ttu-id="78a9c-546">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-546">No</span></span>          | <span data-ttu-id="78a9c-547">Die Genauigkeit der zugehörigen Spalte.</span><span class="sxs-lookup"><span data-stu-id="78a9c-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="78a9c-548">**Scale** (Skalieren)</span><span class="sxs-lookup"><span data-stu-id="78a9c-548">**Scale**</span></span>                 | <span data-ttu-id="78a9c-549">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-549">No</span></span>          | <span data-ttu-id="78a9c-550">Die Dezimalstellenanzahl der zugehörigen Spalte.</span><span class="sxs-lookup"><span data-stu-id="78a9c-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="78a9c-551">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="78a9c-551">**Unicode**</span></span>               | <span data-ttu-id="78a9c-552">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-552">No</span></span>          | <span data-ttu-id="78a9c-553">**"True"** oder **"false"** abhängig davon, ob der entsprechende Spaltenwert als Unicode-Zeichenfolge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="78a9c-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="78a9c-554">**Sortierung**</span><span class="sxs-lookup"><span data-stu-id="78a9c-554">**Collation**</span></span>             | <span data-ttu-id="78a9c-555">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-555">No</span></span>          | <span data-ttu-id="78a9c-556">Eine Zeichenfolge, die die Sortierreihenfolge angibt, die in der Datenquelle verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="78a9c-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="78a9c-557">**SRID**</span><span class="sxs-lookup"><span data-stu-id="78a9c-557">**SRID**</span></span>                  | <span data-ttu-id="78a9c-558">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-558">No</span></span>          | <span data-ttu-id="78a9c-559">Räumliche Verweis Systembezeichner.</span><span class="sxs-lookup"><span data-stu-id="78a9c-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="78a9c-560">Gilt nur für Eigenschaften des räumlichen Typen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="78a9c-561">Weitere Informationen finden Sie unter [SRID](http://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="78a9c-561">For more information, see [SRID](http://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="78a9c-562">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="78a9c-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="78a9c-563">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-563">No</span></span>          | <span data-ttu-id="78a9c-564">**Keine**, **Identität** (ist der entsprechende Spaltenwert eine Identität, die in der Datenbank generiert wird), oder **berechnet** (sofern der entsprechende Spaltenwert in der Datenbank generiert wird).</span><span class="sxs-lookup"><span data-stu-id="78a9c-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="78a9c-565">Nicht gültig für RowType-Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="78a9c-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="78a9c-566">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Eigenschaft** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="78a9c-567">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="78a9c-568">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="78a9c-569">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-569">Example</span></span>

<span data-ttu-id="78a9c-570">Das folgende Beispiel zeigt eine **EntityType** -Element mit zwei untergeordneten **Eigenschaft** Elemente:</span><span class="sxs-lookup"><span data-stu-id="78a9c-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="78a9c-571">PropertyRef-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="78a9c-572">Die **PropertyRef** Element in der Datenspeicherschema-Definitionssprache (SSDL) verweist auf eine Eigenschaft für ein EntityType-Element, um anzugeben, dass die Eigenschaft eine der folgenden Rollen ausführt:</span><span class="sxs-lookup"><span data-stu-id="78a9c-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="78a9c-573">Teil des Primärschlüssels der Tabelle sein, die die **EntityType** darstellt.</span><span class="sxs-lookup"><span data-stu-id="78a9c-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="78a9c-574">Eine oder mehrere **PropertyRef** -Elemente können verwendet werden, um einen Primärschlüssel zu definieren.</span><span class="sxs-lookup"><span data-stu-id="78a9c-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="78a9c-575">Weitere Informationen finden Sie in der Key-Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="78a9c-576">Sie ist das abhängige Ende oder das Prinzipalende einer referenziellen Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="78a9c-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="78a9c-577">Weitere Informationen finden Sie unter ReferentialConstraint-Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="78a9c-578">Die **PropertyRef** -Element kann nur die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="78a9c-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="78a9c-579">Dokumentation (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="78a9c-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="78a9c-580">Anmerkungselemente</span><span class="sxs-lookup"><span data-stu-id="78a9c-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="78a9c-581">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="78a9c-581">Applicable Attributes</span></span>

<span data-ttu-id="78a9c-582">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **PropertyRef** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="78a9c-583">Attributname</span><span class="sxs-lookup"><span data-stu-id="78a9c-583">Attribute Name</span></span> | <span data-ttu-id="78a9c-584">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="78a9c-584">Is Required</span></span> | <span data-ttu-id="78a9c-585">Wert</span><span class="sxs-lookup"><span data-stu-id="78a9c-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="78a9c-586">**Name**</span><span class="sxs-lookup"><span data-stu-id="78a9c-586">**Name**</span></span>       | <span data-ttu-id="78a9c-587">Ja</span><span class="sxs-lookup"><span data-stu-id="78a9c-587">Yes</span></span>         | <span data-ttu-id="78a9c-588">Der Name der referenzierten Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="78a9c-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="78a9c-589">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **PropertyRef** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="78a9c-590">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="78a9c-591">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="78a9c-592">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-592">Example</span></span>

<span data-ttu-id="78a9c-593">Das folgende Beispiel zeigt eine **PropertyRef** Element so definieren Sie einen Primärschlüssel verweisen auf eine Eigenschaft, die auf definiert ist, verwendet eine **EntityType** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

``` xml
 <EntityType Name="Customers">
   <Documentation>
     <Summary>Summary here.</Summary>
     <LongDescription>Long description here.</LongDescription>
   </Documentation>
   <Key>
     <PropertyRef Name="CustomerId" />
   </Key>
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
 </EntityType>
```

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="78a9c-594">ReferentialConstraint-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="78a9c-595">Die **ReferentialConstraint** Element in der Datenspeicherschema-Definitionssprache (SSDL) stellt eine fremdschlüsseleinschränkung (auch Einschränkung der referenziellen Integrität genannt) in der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="78a9c-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="78a9c-596">Das prinzipalende und das abhängige Ende der Einschränkung werden durch die Prinzipal- und die abhängigen untergeordneten Elemente angegeben.</span><span class="sxs-lookup"><span data-stu-id="78a9c-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="78a9c-597">Spalten, die in den Dienstprinzipalnamen und die abhängigen enden beteiligt sind, werden mit PropertyRef-Elemente verwiesen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="78a9c-598">Die **ReferentialConstraint** Element ist ein optionales untergeordnetes Element des Association-Elements.</span><span class="sxs-lookup"><span data-stu-id="78a9c-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="78a9c-599">Wenn eine **ReferentialConstraint** Element wird nicht zum Zuordnen von foreign Key-Einschränkung, die im angegebenen verwendet die **Zuordnung** Element AssociationSetMapping-Element muss zu diesem Zweck verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="78a9c-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="78a9c-600">Die **ReferentialConstraint** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="78a9c-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="78a9c-601">Dokumentation (0 (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="78a9c-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="78a9c-602">Prinzipal (genau ein Element)</span><span class="sxs-lookup"><span data-stu-id="78a9c-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="78a9c-603">Abhängige (genau ein Element)</span><span class="sxs-lookup"><span data-stu-id="78a9c-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="78a9c-604">Anmerkungselemente (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="78a9c-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="78a9c-605">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="78a9c-605">Applicable Attributes</span></span>

<span data-ttu-id="78a9c-606">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **ReferentialConstraint** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="78a9c-607">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="78a9c-608">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="78a9c-609">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-609">Example</span></span>

<span data-ttu-id="78a9c-610">Das folgende Beispiel zeigt eine **Zuordnung** -Element, das verwendet eine **ReferentialConstraint** Element, um die Spalten anzugeben, die Teilnahme an der **FK\_CustomerOrders**  foreign Key-Einschränkung:</span><span class="sxs-lookup"><span data-stu-id="78a9c-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

``` xml
 <Association Name="FK_CustomerOrders">
   <End Role="Customers"
        Type="ExampleModel.Store.Customers" Multiplicity="1">
     <OnDelete Action="Cascade" />
   </End>
   <End Role="Orders"
        Type="ExampleModel.Store.Orders" Multiplicity="*" />
   <ReferentialConstraint>
     <Principal Role="Customers">
       <PropertyRef Name="CustomerId" />
     </Principal>
     <Dependent Role="Orders">
       <PropertyRef Name="CustomerId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```

## <a name="returntype-element-ssdl"></a><span data-ttu-id="78a9c-611">ReturnType-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="78a9c-612">Die **ReturnType** Element in der Datenspeicherschema-Definitionssprache (SSDL) gibt an, den Rückgabetyp für eine Funktion, die in definiert ist eine **Funktion** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="78a9c-613">Eine Funktion, der Rückgabetyp kann auch angegeben werden, mit einem **ReturnType** Attribut.</span><span class="sxs-lookup"><span data-stu-id="78a9c-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="78a9c-614">Der Rückgabetyp einer Funktion wird angegeben, mit der **Typ** Attribut oder das **ReturnType** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="78a9c-615">Die **ReturnType** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="78a9c-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="78a9c-616">CollectionType (eins)</span><span class="sxs-lookup"><span data-stu-id="78a9c-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="78a9c-617">Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **ReturnType** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="78a9c-618">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="78a9c-619">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="78a9c-620">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-620">Example</span></span>

<span data-ttu-id="78a9c-621">Im folgenden Beispiel wird eine **Funktion** , die eine Auflistung von Zeilen zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="78a9c-621">The following example uses a **Function** that returns a collection of rows.</span></span>

``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="78a9c-622">RowType-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="78a9c-623">Ein **RowType** -Element der Datenspeicherschema-Definitionssprache (SSDL) definiert eine unbenannte Struktur als eine Rückgabe für eine Funktion, die im Speicher definiert.</span><span class="sxs-lookup"><span data-stu-id="78a9c-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="78a9c-624">Ein **RowType** Element ist das untergeordnete Element des **CollectionType** Element:</span><span class="sxs-lookup"><span data-stu-id="78a9c-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="78a9c-625">Ein **RowType** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="78a9c-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="78a9c-626">Eigenschaft (ein oder mehrere)</span><span class="sxs-lookup"><span data-stu-id="78a9c-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="78a9c-627">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-627">Example</span></span>

<span data-ttu-id="78a9c-628">Das folgende Beispiel zeigt eine Store-Funktion, verwendet eine **CollectionType** Element, um anzugeben, dass die Funktion eine Auflistung von Zeilen zurückgibt (gemäß den Angaben in der **RowType** Element).</span><span class="sxs-lookup"><span data-stu-id="78a9c-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


``` xml
   <Function Name="GetProducts" IsComposable="true" Schema="dbo">
     <ReturnType>
       <CollectionType>
         <RowType>
           <Property Name="ProductID" Type="int" Nullable="false" />
           <Property Name="CategoryID" Type="bigint" Nullable="false" />
           <Property Name="ProductName" Type="nvarchar" MaxLength="40" Nullable="false" />
           <Property Name="UnitPrice" Type="money" />
           <Property Name="Discontinued" Type="bit" />
         </RowType>
       </CollectionType>
     </ReturnType>
   </Function>
```

## <a name="schema-element-ssdl"></a><span data-ttu-id="78a9c-629">Schema-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="78a9c-630">Die **Schema** Element in der Datenspeicherschema-Definitionssprache (SSDL) ist das Stammelement einer speichermodelldefinition.</span><span class="sxs-lookup"><span data-stu-id="78a9c-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="78a9c-631">Es enthält Definitionen für die Objekte, Funktionen und Container, die zusammen ein Speichermodell bilden.</span><span class="sxs-lookup"><span data-stu-id="78a9c-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="78a9c-632">Die **Schema** -Element kann 0 (null) oder mehrere der folgenden untergeordneten Elemente enthalten:</span><span class="sxs-lookup"><span data-stu-id="78a9c-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="78a9c-633">Zuordnung</span><span class="sxs-lookup"><span data-stu-id="78a9c-633">Association</span></span>
-   <span data-ttu-id="78a9c-634">EntityType</span><span class="sxs-lookup"><span data-stu-id="78a9c-634">EntityType</span></span>
-   <span data-ttu-id="78a9c-635">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="78a9c-635">EntityContainer</span></span>
-   <span data-ttu-id="78a9c-636">Funktion</span><span class="sxs-lookup"><span data-stu-id="78a9c-636">Function</span></span>

<span data-ttu-id="78a9c-637">Die **Schema** -Element verwendet die **Namespace** Attribut, um den Namespace für die Entität und des Zuordnungstyps der Objekte in einem Speichermodell definieren.</span><span class="sxs-lookup"><span data-stu-id="78a9c-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="78a9c-638">Innerhalb eines Namespace müssen alle Objekte eine eindeutige Bezeichnung aufweisen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="78a9c-639">Namespace für ein Speichermodell unterscheidet sich von den XML-Namespace die **Schema** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="78a9c-640">Namespace für ein Speichermodell (gemäß der **Namespace** Attribut) ist ein logischer Container für Entitätstypen und Zuordnungstypen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="78a9c-641">Der XML-Namespace (angegeben durch die **Xmlns** Attribut) des eine **Schema** Element ist der Standardnamespace für untergeordnete Elemente und Attribute des der **Schema** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="78a9c-642">XML-Namespaces im Format http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (, wobei YYYY und MM ein Jahr und Monat darstellen) sind für SSDL reserviert.</span><span class="sxs-lookup"><span data-stu-id="78a9c-642">XML namespaces of the form http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="78a9c-643">Benutzerdefinierte Elemente und Attribute können nicht in Namespaces mit diesem Format vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="78a9c-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="78a9c-644">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="78a9c-644">Applicable Attributes</span></span>

<span data-ttu-id="78a9c-645">Die folgende Tabelle beschreibt die Attribute angewendet werden können, um die **Schema** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="78a9c-646">Attributname</span><span class="sxs-lookup"><span data-stu-id="78a9c-646">Attribute Name</span></span>            | <span data-ttu-id="78a9c-647">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="78a9c-647">Is Required</span></span> | <span data-ttu-id="78a9c-648">Wert</span><span class="sxs-lookup"><span data-stu-id="78a9c-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="78a9c-649">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="78a9c-649">**Namespace**</span></span>             | <span data-ttu-id="78a9c-650">Ja</span><span class="sxs-lookup"><span data-stu-id="78a9c-650">Yes</span></span>         | <span data-ttu-id="78a9c-651">Der Namespace für das Speichermodell.</span><span class="sxs-lookup"><span data-stu-id="78a9c-651">The namespace of the storage model.</span></span> <span data-ttu-id="78a9c-652">Der Wert des der **Namespace** Attribut wird verwendet, um den vollqualifizierten Namen eines Typs zu bilden.</span><span class="sxs-lookup"><span data-stu-id="78a9c-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="78a9c-653">Z. B. wenn ein **EntityType** mit dem Namen *Kunden* befindet sich im Simple.example.Model-Namespace, und klicken Sie dann auf den vollqualifizierten Namen des der **EntityType** ist ExampleModel.Store.Customer.</span><span class="sxs-lookup"><span data-stu-id="78a9c-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="78a9c-654">Die folgenden Zeichenfolgen können nicht verwendet werden, als Wert für die **Namespace** Attribut: **System**, **vorübergehende**, oder **Edm**.</span><span class="sxs-lookup"><span data-stu-id="78a9c-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="78a9c-655">Der Wert für die **Namespace** Attribut kann nicht übereinstimmen, den der Wert für die **Namespace** Attribut im CSDL-Schema-Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="78a9c-656">**Alias**</span><span class="sxs-lookup"><span data-stu-id="78a9c-656">**Alias**</span></span>                 | <span data-ttu-id="78a9c-657">Nein</span><span class="sxs-lookup"><span data-stu-id="78a9c-657">No</span></span>          | <span data-ttu-id="78a9c-658">Ein anstelle der Namespacebezeichnung verwendeter Bezeichner.</span><span class="sxs-lookup"><span data-stu-id="78a9c-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="78a9c-659">Z. B. wenn ein **EntityType** mit dem Namen *Kunden* befindet sich im Simple.example.Model-Namespace und den Wert des der **Alias** Attribut *StorageModel*, dann kann StorageModel.Customer als vollqualifizierter Name des können Sie die **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="78a9c-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="78a9c-660">**Anbieter**</span><span class="sxs-lookup"><span data-stu-id="78a9c-660">**Provider**</span></span>              | <span data-ttu-id="78a9c-661">Ja</span><span class="sxs-lookup"><span data-stu-id="78a9c-661">Yes</span></span>         | <span data-ttu-id="78a9c-662">Der Datenanbieter.</span><span class="sxs-lookup"><span data-stu-id="78a9c-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="78a9c-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="78a9c-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="78a9c-664">Ja</span><span class="sxs-lookup"><span data-stu-id="78a9c-664">Yes</span></span>         | <span data-ttu-id="78a9c-665">Ein Token, das dem Anbieter angibt, welches Anbietermanifest zurückgegeben werden soll.</span><span class="sxs-lookup"><span data-stu-id="78a9c-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="78a9c-666">Für das Token ist kein Format definiert.</span><span class="sxs-lookup"><span data-stu-id="78a9c-666">No format for the token is defined.</span></span> <span data-ttu-id="78a9c-667">Die für das Token möglichen Werte werden vom Anbieter definiert.</span><span class="sxs-lookup"><span data-stu-id="78a9c-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="78a9c-668">Informationen zu SQL Server-Anbietermanifesttoken finden Sie in SqlClient für Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="78a9c-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="78a9c-669">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-669">Example</span></span>

<span data-ttu-id="78a9c-670">Das folgende Beispiel zeigt eine **Schema** -Element mit einem **EntityContainer** Element, zwei **EntityType** -Elemente und ein **Zuordnung** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
   <EntityContainer Name="ExampleModelStoreContainer">
     <EntitySet Name="Customers"
                EntityType="ExampleModel.Store.Customers"
                Schema="dbo" />
     <EntitySet Name="Orders"
                EntityType="ExampleModel.Store.Orders"
                Schema="dbo" />
     <AssociationSet Name="FK_CustomerOrders"
                     Association="ExampleModel.Store.FK_CustomerOrders">
       <End Role="Customers" EntitySet="Customers" />
       <End Role="Orders" EntitySet="Orders" />
     </AssociationSet>
   </EntityContainer>
   <EntityType Name="Customers">
     <Documentation>
       <Summary>Summary here.</Summary>
       <LongDescription>Long description here.</LongDescription>
     </Documentation>
     <Key>
       <PropertyRef Name="CustomerId" />
     </Key>
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <Property Name="Name" Type="nvarchar(max)" Nullable="false" />
   </EntityType>
   <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
     <Key>
       <PropertyRef Name="OrderId" />
     </Key>
     <Property Name="OrderId" Type="int" Nullable="false"
               c:CustomAttribute="someValue"/>
     <Property Name="ProductId" Type="int" Nullable="false" />
     <Property Name="Quantity" Type="int" Nullable="false" />
     <Property Name="CustomerId" Type="int" Nullable="false" />
     <c:CustomElement>
       Custom data here.
     </c:CustomElement>
   </EntityType>
   <Association Name="FK_CustomerOrders">
     <End Role="Customers"
          Type="ExampleModel.Store.Customers" Multiplicity="1">
       <OnDelete Action="Cascade" />
     </End>
     <End Role="Orders"
          Type="ExampleModel.Store.Orders" Multiplicity="*" />
     <ReferentialConstraint>
       <Principal Role="Customers">
         <PropertyRef Name="CustomerId" />
       </Principal>
       <Dependent Role="Orders">
         <PropertyRef Name="CustomerId" />
       </Dependent>
     </ReferentialConstraint>
   </Association>
   <Function Name="UpdateOrderQuantity"
             Aggregate="false"
             BuiltIn="false"
             NiladicFunction="false"
             IsComposable="false"
             ParameterTypeSemantics="AllowImplicitConversion"
             Schema="dbo">
     <Parameter Name="orderId" Type="int" Mode="In" />
     <Parameter Name="newQuantity" Type="int" Mode="In" />
   </Function>
   <Function Name="UpdateProductInOrder" IsComposable="false">
     <CommandText>
       UPDATE Orders
       SET ProductId = @productId
       WHERE OrderId = @orderId;
     </CommandText>
     <Parameter Name="productId"
                Mode="In"
                Type="int"/>
     <Parameter Name="orderId"
                Mode="In"
                Type="int"/>
   </Function>
 </Schema>
```

## <a name="annotation-attributes"></a><span data-ttu-id="78a9c-671">Anmerkungsattribute</span><span class="sxs-lookup"><span data-stu-id="78a9c-671">Annotation Attributes</span></span>

<span data-ttu-id="78a9c-672">Anmerkungsattribute sind in der Datenspeicherschema-Definitionssprache (Store Schema Definition Language, SSDL) benutzerdefinierte XML-Attribute im Speichermodell, die zusätzliche Metadaten zu den Elementen im Speichermodell bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="78a9c-673">Neben dem Vorhandensein einer gültigen XML-Struktur gelten für Anmerkungsattribute folgende Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="78a9c-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="78a9c-674">Anmerkungsattribute dürfen sich in keinem XML-Namespace befinden, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="78a9c-675">Die vollqualifizierten Namen zweier Anmerkungsattribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="78a9c-676">Für ein angegebenes SSDL-Element kann mehr als ein Anmerkungsattribut übernommen werden.</span><span class="sxs-lookup"><span data-stu-id="78a9c-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="78a9c-677">Metadaten in Anmerkungselementen kann zur Laufzeit zugegriffen werden, mithilfe von Klassen im Namespace System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="78a9c-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="78a9c-678">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-678">Example</span></span>

<span data-ttu-id="78a9c-679">Das folgende Beispiel zeigt ein EntityType-Element, das ein anmerkungsattribut übernommen verfügt die **"OrderID"** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="78a9c-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="78a9c-680">Das Beispiel zeigt außerdem ein Anmerkungselement, das hinzugefügt, die **EntityType** Element.</span><span class="sxs-lookup"><span data-stu-id="78a9c-680">The example also show an annotation element added to the **EntityType** element.</span></span>

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="78a9c-681">Anmerkungelemente (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="78a9c-682">Anmerkungselemente sind in der Datenspeicherschema-Definitionssprache (Store Schema Definition Language, SSDL) benutzerdefinierte XML-Elemente im Speichermodell, die zusätzliche Metadaten zum Speichermodell bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="78a9c-683">Neben dem Vorhandensein einer gültigen XML-Struktur gelten für Anmerkungselemente folgende Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="78a9c-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="78a9c-684">Anmerkungselemente dürfen sich in keinem XML-Namespace befinden, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="78a9c-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="78a9c-685">Die vollqualifizierten Namen zweier Anmerkungselemente dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="78a9c-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="78a9c-686">Anmerkungselemente müssen nach allen anderen untergeordneten Elementen eines angegebenen SSDL-Elements angeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="78a9c-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="78a9c-687">Mehrere Anmerkungselemente können untergeordnete Elemente eines angegebenen SSDL-Elements sein.</span><span class="sxs-lookup"><span data-stu-id="78a9c-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="78a9c-688">Ab .NET Framework, Version 4, kann in Anmerkungselementen enthaltene Metadaten zur Laufzeit zugegriffen werden mithilfe von Klassen im Namespace System.Data.Metadata.Edm.</span><span class="sxs-lookup"><span data-stu-id="78a9c-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="78a9c-689">Beispiel</span><span class="sxs-lookup"><span data-stu-id="78a9c-689">Example</span></span>

<span data-ttu-id="78a9c-690">Das folgende Beispiel zeigt ein EntityType-Element, das ein Annotation-Element hat (**CustomElement**).</span><span class="sxs-lookup"><span data-stu-id="78a9c-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="78a9c-691">Das Beispiel zeigt außerdem ein anmerkungsattribut übernommen werden, um die **"OrderID"** Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="78a9c-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

``` xml
 <EntityType Name="Orders" xmlns:c="http://CustomNamespace">
   <Key>
     <PropertyRef Name="OrderId" />
   </Key>
   <Property Name="OrderId" Type="int" Nullable="false"
             c:CustomAttribute="someValue"/>
   <Property Name="ProductId" Type="int" Nullable="false" />
   <Property Name="Quantity" Type="int" Nullable="false" />
   <Property Name="CustomerId" Type="int" Nullable="false" />
   <c:CustomElement>
     Custom data here.
   </c:CustomElement>
 </EntityType>
```

## <a name="facets-ssdl"></a><span data-ttu-id="78a9c-692">Facets (SSDL)</span><span class="sxs-lookup"><span data-stu-id="78a9c-692">Facets (SSDL)</span></span>

<span data-ttu-id="78a9c-693">Facets in Datenspeicherschema-Definitionssprache (SSDL) stellen Einschränkungen für Spaltentypen, die in Eigenschaftenelementen angegeben sind dar.</span><span class="sxs-lookup"><span data-stu-id="78a9c-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="78a9c-694">Facets werden als XML-Attribute auf implementiert **Eigenschaft** Elemente.</span><span class="sxs-lookup"><span data-stu-id="78a9c-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="78a9c-695">In der folgenden Tabelle werden die in SSDL unterstützten Facets beschrieben:</span><span class="sxs-lookup"><span data-stu-id="78a9c-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="78a9c-696">Facet</span><span class="sxs-lookup"><span data-stu-id="78a9c-696">Facet</span></span>           | <span data-ttu-id="78a9c-697">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="78a9c-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="78a9c-698">**Sortierung**</span><span class="sxs-lookup"><span data-stu-id="78a9c-698">**Collation**</span></span>   | <span data-ttu-id="78a9c-699">Gibt die bei Vergleich- und Sortiervorgängen zu verwendende Sortierreihenfolge für die Werte der Eigenschaft an.</span><span class="sxs-lookup"><span data-stu-id="78a9c-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="78a9c-700">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="78a9c-700">**FixedLength**</span></span> | <span data-ttu-id="78a9c-701">Gibt an, ob sich die Länge des Spaltenwerts ändern kann.</span><span class="sxs-lookup"><span data-stu-id="78a9c-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="78a9c-702">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="78a9c-702">**MaxLength**</span></span>   | <span data-ttu-id="78a9c-703">Gibt die maximale Länge des Spaltenwerts an.</span><span class="sxs-lookup"><span data-stu-id="78a9c-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="78a9c-704">**Genauigkeit**</span><span class="sxs-lookup"><span data-stu-id="78a9c-704">**Precision**</span></span>   | <span data-ttu-id="78a9c-705">Bei Eigenschaften vom Typ **Decimal**, gibt die Anzahl der Ziffern ein Eigenschaftswert verfügen kann.</span><span class="sxs-lookup"><span data-stu-id="78a9c-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="78a9c-706">Bei Eigenschaften vom Typ **Zeit**, **"DateTime"**, und **DateTimeOffset**, gibt die Anzahl von Ziffern für den Bruchteil der Sekunden des Spaltenwerts.</span><span class="sxs-lookup"><span data-stu-id="78a9c-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="78a9c-707">**Scale** (Skalieren)</span><span class="sxs-lookup"><span data-stu-id="78a9c-707">**Scale**</span></span>       | <span data-ttu-id="78a9c-708">Gibt die Anzahl der Dezimalstellen für den Spaltenwert an.</span><span class="sxs-lookup"><span data-stu-id="78a9c-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="78a9c-709">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="78a9c-709">**Unicode**</span></span>     | <span data-ttu-id="78a9c-710">Gibt an, ob der Spaltenwert als Unicode gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="78a9c-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
