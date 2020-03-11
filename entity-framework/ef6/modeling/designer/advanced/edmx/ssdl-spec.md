---
title: SSDL-Spezifikation-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: b20d1f99f1da9c53a8a164fccc461e07d19c879d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415472"
---
# <a name="ssdl-specification"></a><span data-ttu-id="2c2fe-102">SSDL-Spezifikation</span><span class="sxs-lookup"><span data-stu-id="2c2fe-102">SSDL Specification</span></span>
<span data-ttu-id="2c2fe-103">Die Datenspeicherschema-Definitionssprache (Store Schema Definition Language, SSDL) ist eine XML-basierte Sprache, die das Speichermodell einer Entity Framework-Anwendung beschreibt.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-103">Store schema definition language (SSDL) is an XML-based language that describes the storage model of an Entity Framework application.</span></span>

<span data-ttu-id="2c2fe-104">In einer Entity Framework Anwendung werden die Metadaten des Speicher Modells aus einer SSDL-Datei (geschrieben in SSDL) in eine Instanz der System. Data. Metadata. Edm. StoreItemCollection geladen und können mithilfe von Methoden im System. Data. Metadata. Edm. MetadataWorkspace-Klasse.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-104">In an Entity Framework application, storage model metadata is loaded from a .ssdl file (written in SSDL) into an instance of the System.Data.Metadata.Edm.StoreItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="2c2fe-105">Entity Framework verwendet Metadaten des Speicher Modells, um Abfragen für das konzeptionelle Modell in Speicher spezifische Befehle zu übersetzen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-105">Entity Framework uses storage model metadata to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="2c2fe-106">Der Entity Framework Designer (EF-Designer) speichert Speichermodell Informationen in einer EDMX-Datei zur Entwurfszeit.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-106">The Entity Framework Designer (EF Designer) stores storage model information in an .edmx file at design time.</span></span> <span data-ttu-id="2c2fe-107">Zum Zeitpunkt der Erstellung verwendet der Entity Designer Informationen in einer EDMX-Datei, um die SSDL-Datei zu erstellen, die zur Laufzeit von Entity Framework benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-107">At build time the Entity Designer uses information in an .edmx file to create the .ssdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="2c2fe-108">Die verschiedenen Versionen von SSDL werden durch XML-Namespaces unterschieden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-108">Versions of SSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="2c2fe-109">SSDL-Version</span><span class="sxs-lookup"><span data-stu-id="2c2fe-109">SSDL Version</span></span> | <span data-ttu-id="2c2fe-110">XML-Namespace</span><span class="sxs-lookup"><span data-stu-id="2c2fe-110">XML Namespace</span></span>                                     |
|:-------------|:--------------------------------------------------|
| <span data-ttu-id="2c2fe-111">SSDL v1</span><span class="sxs-lookup"><span data-stu-id="2c2fe-111">SSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| <span data-ttu-id="2c2fe-112">SSDL v2</span><span class="sxs-lookup"><span data-stu-id="2c2fe-112">SSDL v2</span></span>      | https://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| <span data-ttu-id="2c2fe-113">SSDL v3</span><span class="sxs-lookup"><span data-stu-id="2c2fe-113">SSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a><span data-ttu-id="2c2fe-114">Zuordnungselement (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-114">Association Element (SSDL)</span></span>

<span data-ttu-id="2c2fe-115">Ein **Association** -Element in der Datenspeicher Schema-Definitions Sprache (SSDL) gibt Tabellen Spalten an, die an einer FOREIGN KEY-Einschränkung in der zugrunde liegenden Datenbank beteiligt sind.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-115">An **Association** element in store schema definition language (SSDL) specifies table columns that participate in a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="2c2fe-116">Zwei erforderliche untergeordnete End-Elemente geben die Tabellen an den Enden der Zuordnung und die Multiplizität an jedem Ende an.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-116">Two required child End elements specify tables at the ends of the association and the multiplicity at each end.</span></span> <span data-ttu-id="2c2fe-117">Ein optionales ReferentialConstraint-Element gibt die Prinzipal- und die abhängigen Enden der Zuordnung sowie die teilnehmenden Spalten an.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-117">An optional ReferentialConstraint element specifies the principal and dependent ends of the association as well as the participating columns.</span></span> <span data-ttu-id="2c2fe-118">Wenn kein **referentialeinschränkungselement** vorhanden ist, muss ein AssociationSetMapping-Element verwendet werden, um die Spalten Zuordnungen für die Zuordnung anzugeben.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-118">If no **ReferentialConstraint** element is present, an AssociationSetMapping element must be used to specify the column mappings for the association.</span></span>

<span data-ttu-id="2c2fe-119">Das **Association** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="2c2fe-119">The **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2c2fe-120">Dokumentation (0 (null) oder 1)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-120">Documentation (zero or one)</span></span>
-   <span data-ttu-id="2c2fe-121">Ende (genau zwei)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-121">End (exactly two)</span></span>
-   <span data-ttu-id="2c2fe-122">Referenentialeinschränkung (null oder 1)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-122">ReferentialConstraint (zero or one)</span></span>
-   <span data-ttu-id="2c2fe-123">Anmerkungselemente (kein (null) oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-123">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2c2fe-124">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="2c2fe-124">Applicable Attributes</span></span>

<span data-ttu-id="2c2fe-125">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **Association** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-125">The following table describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="2c2fe-126">Attributname</span><span class="sxs-lookup"><span data-stu-id="2c2fe-126">Attribute Name</span></span> | <span data-ttu-id="2c2fe-127">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="2c2fe-127">Is Required</span></span> | <span data-ttu-id="2c2fe-128">value</span><span class="sxs-lookup"><span data-stu-id="2c2fe-128">Value</span></span>                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| <span data-ttu-id="2c2fe-129">**Name**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-129">**Name**</span></span>       | <span data-ttu-id="2c2fe-130">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2fe-130">Yes</span></span>         | <span data-ttu-id="2c2fe-131">Der Name der entsprechenden Fremdschlüsseleinschränkung in der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-131">The name of the corresponding foreign key constraint in the underlying database.</span></span> |

> [!NOTE]
> <span data-ttu-id="2c2fe-132">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **Association** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-132">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="2c2fe-133">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-133">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="2c2fe-134">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-134">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="2c2fe-135">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-135">Example</span></span>

<span data-ttu-id="2c2fe-136">Das folgende Beispiel zeigt ein **Association** -Element, das ein **referentialeinschränkung** -Element verwendet, um die Spalten anzugeben, die an der **FK-\_CustomerOrders** -Foreign Key-Einschränkung beteiligt sind:</span><span class="sxs-lookup"><span data-stu-id="2c2fe-136">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="associationset-element-ssdl"></a><span data-ttu-id="2c2fe-137">AssociationSet-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-137">AssociationSet Element (SSDL)</span></span>

<span data-ttu-id="2c2fe-138">Das **AssociationSet** -Element in der Datenspeicher Schema-Definitions Sprache (SSDL) stellt eine FOREIGN KEY-Einschränkung zwischen zwei Tabellen in der zugrunde liegenden Datenbank dar.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-138">The **AssociationSet** element in store schema definition language (SSDL) represents a foreign key constraint between two tables in the underlying database.</span></span> <span data-ttu-id="2c2fe-139">Die Tabellenspalten, die an der Fremdschlüsseleinschränkung teilnehmen, werden in einem Association-Element angegeben.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-139">The table columns that participate in the foreign key constraint are specified in an Association element.</span></span> <span data-ttu-id="2c2fe-140">Das **Association** -Element, das einem bestimmten **AssociationSet** -Element entspricht, wird im **Association** -Attribut des **AssociationSet** -Elements angegeben.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-140">The **Association** element that corresponds to a given **AssociationSet** element is specified in the **Association** attribute of the **AssociationSet** element.</span></span>

<span data-ttu-id="2c2fe-141">SSDL-Zuordnungssätze werden CSDL-Zuordnungssätzen durch ein AssociationSetMapping-Element zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-141">SSDL association sets are mapped to CSDL association sets by an AssociationSetMapping element.</span></span> <span data-ttu-id="2c2fe-142">Wenn jedoch die CSDL-Zuordnung für einen bestimmten CSDL-Zuordnungs Satz mithilfe eines referentialeinschränkung-Elements definiert wird, ist kein entsprechendes **AssociationSetMapping** -Element erforderlich.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-142">However, if the CSDL association for a given CSDL association set is defined by using a ReferentialConstraint element , no corresponding **AssociationSetMapping** element is necessary.</span></span> <span data-ttu-id="2c2fe-143">Wenn in diesem Fall ein **AssociationSetMapping** -Element vorhanden ist, werden die von ihm definierten Zuordnungen durch das **referentialeinschränkung** -Element überschrieben.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-143">In this case, if an **AssociationSetMapping** element is present, the mappings it defines will be overridden by the **ReferentialConstraint** element.</span></span>

<span data-ttu-id="2c2fe-144">Das **AssociationSet** -Element kann über die folgenden untergeordneten Elemente verfügen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="2c2fe-144">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2c2fe-145">Dokumentation (0 (null) oder 1)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-145">Documentation (zero or one)</span></span>
-   <span data-ttu-id="2c2fe-146">End (kein (null) oder zwei Elemente)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-146">End (zero or two)</span></span>
-   <span data-ttu-id="2c2fe-147">Anmerkungselemente (kein (null) oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-147">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2c2fe-148">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="2c2fe-148">Applicable Attributes</span></span>

<span data-ttu-id="2c2fe-149">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **AssociationSet** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-149">The following table describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="2c2fe-150">Attributname</span><span class="sxs-lookup"><span data-stu-id="2c2fe-150">Attribute Name</span></span>  | <span data-ttu-id="2c2fe-151">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="2c2fe-151">Is Required</span></span> | <span data-ttu-id="2c2fe-152">value</span><span class="sxs-lookup"><span data-stu-id="2c2fe-152">Value</span></span>                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2c2fe-153">**Name**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-153">**Name**</span></span>        | <span data-ttu-id="2c2fe-154">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2fe-154">Yes</span></span>         | <span data-ttu-id="2c2fe-155">Der Name der Fremdschlüsseleinschränkung, die der Zuordnungssatz darstellt.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-155">The name of the foreign key constraint that the association set represents.</span></span>                          |
| <span data-ttu-id="2c2fe-156">**Anwalt**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-156">**Association**</span></span> | <span data-ttu-id="2c2fe-157">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2fe-157">Yes</span></span>         | <span data-ttu-id="2c2fe-158">Der Name der Zuordnung, die die Spalten definiert, die an der Fremdschlüsseleinschränkung teilnehmen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-158">The name of the association that defines the columns that participate in the foreign key constraint.</span></span> |

> [!NOTE]
> <span data-ttu-id="2c2fe-159">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **AssociationSet** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-159">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="2c2fe-160">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-160">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="2c2fe-161">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-161">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="2c2fe-162">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-162">Example</span></span>

<span data-ttu-id="2c2fe-163">Das folgende Beispiel zeigt ein **AssociationSet** -Element, das die `FK_CustomerOrders` Foreign Key-Einschränkung in der zugrunde liegenden Datenbank darstellt:</span><span class="sxs-lookup"><span data-stu-id="2c2fe-163">The following example shows an **AssociationSet** element that represents the `FK_CustomerOrders` foreign key constraint in the underlying database:</span></span>

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a><span data-ttu-id="2c2fe-164">CollectionType-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-164">CollectionType Element (SSDL)</span></span>

<span data-ttu-id="2c2fe-165">Das **CollectionType** -Element in der Speicher Schema-Definitions Sprache (SSDL) gibt an, dass der Rückgabetyp einer Funktion eine Auflistung ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-165">The **CollectionType** element in store schema definition language (SSDL) specifies that a function’s return type is a collection.</span></span> <span data-ttu-id="2c2fe-166">Das **CollectionType** -Element ist ein untergeordnetes Element des ReturnType-Elements.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-166">The **CollectionType** element is a child of the ReturnType element.</span></span> <span data-ttu-id="2c2fe-167">Der Sammlungstyp wird mithilfe des untergeordneten RowType-Elements angegeben:</span><span class="sxs-lookup"><span data-stu-id="2c2fe-167">The type of collection is specified by using the RowType child element:</span></span>

> [!NOTE]
> <span data-ttu-id="2c2fe-168">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **CollectionType** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-168">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="2c2fe-169">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-169">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="2c2fe-170">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-170">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="2c2fe-171">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-171">Example</span></span>

<span data-ttu-id="2c2fe-172">Das folgende Beispiel zeigt eine Funktion, die ein **CollectionType** -Element verwendet, um anzugeben, dass die Funktion eine Auflistung von Zeilen zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-172">The following example shows a function that uses a **CollectionType** element to specify that the function returns a collection of rows.</span></span>

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

## <a name="commandtext-element-ssdl"></a><span data-ttu-id="2c2fe-173">CommandText-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-173">CommandText Element (SSDL)</span></span>

<span data-ttu-id="2c2fe-174">Das **CommandText** -Element in der Datenspeicher Schema-Definitions Sprache (SSDL) ist ein untergeordnetes Element des Function-Elements, mit dem Sie eine SQL-Anweisung definieren können, die in der Datenbank ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-174">The **CommandText** element in store schema definition language (SSDL) is a child of the Function element that allows you to define a SQL statement that is executed at the database.</span></span> <span data-ttu-id="2c2fe-175">Mit dem **CommandText** -Element können Sie Funktionen hinzufügen, die einer gespeicherten Prozedur in der-Datenbank ähneln, aber Sie definieren das **CommandText** -Element im Speichermodell.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-175">The **CommandText** element allows you to add functionality that is similar to a stored procedure in the database, but you define the **CommandText** element in the storage model.</span></span>

<span data-ttu-id="2c2fe-176">Das **CommandText** -Element darf keine untergeordneten Elemente aufweisen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-176">The **CommandText** element cannot have child elements.</span></span> <span data-ttu-id="2c2fe-177">Der **Text des CommandText** -Elements muss eine gültige SQL-Anweisung für die zugrunde liegende Datenbank sein.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-177">The body of the **CommandText** element must be a valid SQL statement for the underlying database.</span></span>

<span data-ttu-id="2c2fe-178">Für das **CommandText** -Element sind keine Attribute anwendbar.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-178">No attributes are applicable to the **CommandText** element.</span></span>

### <a name="example"></a><span data-ttu-id="2c2fe-179">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-179">Example</span></span>

<span data-ttu-id="2c2fe-180">Das folgende Beispiel zeigt ein **Function** -Element mit einem untergeordneten **CommandText** -Element.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-180">The following example shows a **Function** element with a child **CommandText** element.</span></span> <span data-ttu-id="2c2fe-181">Machen Sie die Funktion **updateproductinorder** als Methode für ObjectContext verfügbar, indem Sie Sie in das konzeptionelle Modell importieren.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-181">Expose the **UpdateProductInOrder** function as a method on the ObjectContext by importing it into the conceptual model.</span></span>  

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

## <a name="definingquery-element-ssdl"></a><span data-ttu-id="2c2fe-182">DefiningQuery-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-182">DefiningQuery Element (SSDL)</span></span>

<span data-ttu-id="2c2fe-183">Mit dem **DefiningQuery** -Element in der Datenspeicher Schema-Definitions Sprache (SSDL) können Sie eine SQL-Anweisung direkt in der zugrunde liegenden Datenbank ausführen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-183">The **DefiningQuery** element in store schema definition language (SSDL) allows you to execute a SQL statement directly in the underlying database.</span></span> <span data-ttu-id="2c2fe-184">Das **DefiningQuery** -Element wird häufig wie eine Daten Bank Sicht verwendet, aber die Sicht wird im Speichermodell anstelle der Datenbank definiert.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-184">The **DefiningQuery** element is commonly used like a database view, but the view is defined in the storage model instead of the database.</span></span> <span data-ttu-id="2c2fe-185">Die in einem **DefiningQuery** -Element definierte Sicht kann mithilfe eines EntitySetMapping-Elements einem Entitätstyp im konzeptionellen Modell zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-185">The view defined in a **DefiningQuery** element can be mapped to an entity type in the conceptual model through an EntitySetMapping element.</span></span> <span data-ttu-id="2c2fe-186">Diese Zuordnungen sind schreibgeschützt.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-186">These mappings are read-only.</span></span>  

<span data-ttu-id="2c2fe-187">Die folgende SSDL-Syntax zeigt die Deklaration eines **EntitySet** , gefolgt vom **DefiningQuery** -Element, das eine Abfrage enthält, die zum Abrufen der Sicht verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-187">The following SSDL syntax shows the declaration of an **EntitySet** followed by the **DefiningQuery** element that contains a query used to retrieve the view.</span></span>

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

<span data-ttu-id="2c2fe-188">Sie können gespeicherte Prozeduren in der Entity Framework verwenden, um Szenarien mit Lese-/Schreibzugriff zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-188">You can use stored procedures in the Entity Framework to enable read-write scenarios over views.</span></span><span data-ttu-id="2c2fe-189"> Sie können entweder eine Datenquellen Sicht oder eine Entity SQL Sicht als Basistabelle zum Abrufen von Daten und zur Änderungs Verarbeitung durch gespeicherte Prozeduren verwenden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-189"> You can use either a data source view or an Entity SQL view as the base table for retrieving data and for change processing by stored procedures.</span></span>

<span data-ttu-id="2c2fe-190">Sie können das **DefiningQuery** -Element verwenden, um Microsoft SQL Server Compact 3,5-Ziel zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-190">You can use the **DefiningQuery** element to target Microsoft SQL Server Compact 3.5.</span></span> <span data-ttu-id="2c2fe-191">Obwohl SQL Server Compact 3,5 keine gespeicherten Prozeduren unterstützt, können Sie eine ähnliche Funktionalität mit dem **DefiningQuery** -Element implementieren.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-191">Though SQL Server Compact 3.5 does not support stored procedures, you can implement similar functionality with the **DefiningQuery** element.</span></span> <span data-ttu-id="2c2fe-192">Es kann auch beim Erstellen gespeicherter Prozeduren nützlich sein, um einen Konflikt zwischen den in der Programmiersprache und den in der Datenquelle verwendeten Datentypen zu lösen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-192">Another place where it can be useful is in creating stored procedures to overcome a mismatch between the data types used in the programming language and those of the data source.</span></span> <span data-ttu-id="2c2fe-193">Sie könnten eine **DefiningQuery** schreiben, die einen bestimmten Satz von Parametern annimmt, und dann eine gespeicherte Prozedur mit einem anderen Satz von Parametern aufrufen, z. b. eine gespeicherte Prozedur, die Daten löscht.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-193">You could write a **DefiningQuery** that takes a certain set of parameters and then calls a stored procedure with a different set of parameters, for example, a stored procedure that deletes data.</span></span>

## <a name="dependent-element-ssdl"></a><span data-ttu-id="2c2fe-194">Abhängiges Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-194">Dependent Element (SSDL)</span></span>

<span data-ttu-id="2c2fe-195">Das **abhängige** Element in der Speicher Schema-Definitions Sprache (Store Schema Definition Language, SSDL) ist ein untergeordnetes Element des referentialeinschränkung-Elements, das das abhängige Ende einer FOREIGN KEY-Einschränkung definiert (auch referenzielle Einschränkung genannt).</span><span class="sxs-lookup"><span data-stu-id="2c2fe-195">The **Dependent** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the dependent end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="2c2fe-196">Das **abhängige** Element gibt die Spalte (oder Spalten) in einer Tabelle an, die auf eine Primärschlüssel Spalte (oder Spalten) verweisen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-196">The **Dependent** element specifies the column (or columns) in a table that reference a primary key column (or columns).</span></span> <span data-ttu-id="2c2fe-197">**PropertyRef** -Elemente geben an, auf welche Spalten verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-197">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="2c2fe-198">Das Principal-Element gibt die Primärschlüssel Spalten an, auf die von Spalten verwiesen wird, die im **abhängigen** Element angegeben sind.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-198">The Principal element specifies the primary key columns that are referenced by columns that are specified in the **Dependent** element.</span></span>

<span data-ttu-id="2c2fe-199">Das **abhängige** Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="2c2fe-199">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2c2fe-200">PropertyRef (ein oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-200">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="2c2fe-201">Anmerkungselemente (kein (null) oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-201">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2c2fe-202">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="2c2fe-202">Applicable Attributes</span></span>

<span data-ttu-id="2c2fe-203">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **abhängige** Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-203">The following table describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="2c2fe-204">Attributname</span><span class="sxs-lookup"><span data-stu-id="2c2fe-204">Attribute Name</span></span> | <span data-ttu-id="2c2fe-205">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="2c2fe-205">Is Required</span></span> | <span data-ttu-id="2c2fe-206">value</span><span class="sxs-lookup"><span data-stu-id="2c2fe-206">Value</span></span>                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2c2fe-207">**Rolle**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-207">**Role**</span></span>       | <span data-ttu-id="2c2fe-208">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2fe-208">Yes</span></span>         | <span data-ttu-id="2c2fe-209">Der gleiche Wert wie das **Role** -Attribut (sofern verwendet) des entsprechenden End-Elements. andernfalls der Name der Tabelle, die die verweisende Spalte enthält.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-209">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referencing column.</span></span> |

> [!NOTE]
> <span data-ttu-id="2c2fe-210">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **abhängige** Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-210">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="2c2fe-211">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-211">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2c2fe-212">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-212">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="2c2fe-213">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-213">Example</span></span>

<span data-ttu-id="2c2fe-214">Das folgende Beispiel zeigt ein Association-Element, das ein **referentialeinschränkung** -Element verwendet, um die Spalten anzugeben, die an der **FK-\_CustomerOrders** -Fremdschlüssel Einschränkung teilnehmen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-214">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="2c2fe-215">Das **abhängige** Element gibt die **CustomerID-** Spalte der **Order** -Tabelle als abhängiges Ende der Einschränkung an.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-215">The **Dependent** element specifies the **CustomerId** column of the **Order** table as the dependent end of the constraint.</span></span>

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

## <a name="documentation-element-ssdl"></a><span data-ttu-id="2c2fe-216">Documentation-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-216">Documentation Element (SSDL)</span></span>

<span data-ttu-id="2c2fe-217">Das **Dokumentations** Element in der Datenspeicher Schema-Definitions Sprache (Store Schema Definition Language, SSDL) kann verwendet werden, um Informationen zu einem Objekt bereitzustellen, das in einem übergeordneten Element definiert</span><span class="sxs-lookup"><span data-stu-id="2c2fe-217">The **Documentation** element in store schema definition language (SSDL) can be used to provide information about an object that is defined in a parent element.</span></span>

<span data-ttu-id="2c2fe-218">Das **Documentation** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="2c2fe-218">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2c2fe-219">**Zusammenfassung**: eine kurze Beschreibung des übergeordneten Elements.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-219">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="2c2fe-220">(kein (Null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-220">(zero or one element)</span></span>
-   <span data-ttu-id="2c2fe-221">**LongDescription**: eine umfassende Beschreibung des übergeordneten Elements.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-221">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="2c2fe-222">(kein (Null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-222">(zero or one element)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2c2fe-223">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="2c2fe-223">Applicable Attributes</span></span>

<span data-ttu-id="2c2fe-224">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **Documentation** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-224">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="2c2fe-225">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-225">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2c2fe-226">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-226">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="2c2fe-227">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-227">Example</span></span>

<span data-ttu-id="2c2fe-228">Das folgende Beispiel zeigt das **Documentation** -Element als untergeordnetes Element eines EntityType-Elements.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-228">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span>

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

## <a name="end-element-ssdl"></a><span data-ttu-id="2c2fe-229">End-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-229">End Element (SSDL)</span></span>

<span data-ttu-id="2c2fe-230">Das **End** -Element in der Datenspeicher Schema-Definitions Sprache (Store Schema Definition Language, SSDL) gibt die Tabelle und die Anzahl der Zeilen an einem Ende einer Fremdschlüssel Einschränkung in der zugrunde liegenden Datenbank an.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-230">The **End** element in store schema definition language (SSDL) specifies the table and number of rows at one end of a foreign key constraint in the underlying database.</span></span> <span data-ttu-id="2c2fe-231">Das **End** -Element kann ein untergeordnetes Element des Association-Elements oder des AssociationSet-Elements sein.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-231">The **End** element can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="2c2fe-232">In jedem dieser Fälle sind andere untergeordnete Elemente und anwendbare Attribute die möglich.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-232">In each case, the possible child elements and applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="2c2fe-233">Das End-Element als untergeordnetes Objekt des Association-Elements</span><span class="sxs-lookup"><span data-stu-id="2c2fe-233">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="2c2fe-234">Ein **End** -Element (als untergeordnetes Element des **Association** -Elements) gibt die Tabelle und die Anzahl der Zeilen am Ende einer Foreign **Key-Einschränkung** mit dem **Type** -Attribut und dem Multiplizitätsattribut an.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-234">An **End** element (as a child of the **Association** element) specifies the table and number of rows at the end of a foreign key constraint with the **Type** and **Multiplicity** attributes respectively.</span></span> <span data-ttu-id="2c2fe-235">Die Enden einer Fremdschlüsseleinschränkung sind als Teil einer SSDL-Zuordnung definiert. Eine SSDL-Zuordnung muss genau zwei Enden aufweisen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-235">Ends of a foreign key constraint are defined as part of an SSDL association; an SSDL association must have exactly two ends.</span></span>

<span data-ttu-id="2c2fe-236">Ein **Endelement** kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="2c2fe-236">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2c2fe-237">Dokumentation (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-237">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="2c2fe-238">OnDelete (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-238">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="2c2fe-239">Annotation-Elemente (0 (null) oder mehr Elemente)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-239">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="2c2fe-240">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="2c2fe-240">Applicable Attributes</span></span>

<span data-ttu-id="2c2fe-241">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **End** -Element angewendet werden können, wenn es sich um das untergeordnete Element eines **Association** -Elements handelt.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-241">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="2c2fe-242">Attributname</span><span class="sxs-lookup"><span data-stu-id="2c2fe-242">Attribute Name</span></span>   | <span data-ttu-id="2c2fe-243">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="2c2fe-243">Is Required</span></span> | <span data-ttu-id="2c2fe-244">value</span><span class="sxs-lookup"><span data-stu-id="2c2fe-244">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2c2fe-245">**Typ**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-245">**Type**</span></span>         | <span data-ttu-id="2c2fe-246">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2fe-246">Yes</span></span>         | <span data-ttu-id="2c2fe-247">Der vollqualifizierte Name der SSDL-Entitätenmenge, die sich am Ende der Fremdschlüsseleinschränkung befindet.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-247">The fully qualified name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                                                                                                                                                                                                                                                                          |
| <span data-ttu-id="2c2fe-248">**Rolle**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-248">**Role**</span></span>         | <span data-ttu-id="2c2fe-249">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-249">No</span></span>          | <span data-ttu-id="2c2fe-250">Der Wert des **Role** -Attributs im Prinzipal-oder abhängigen Element des entsprechenden referentialeinschränkung-Elements (sofern verwendet).</span><span class="sxs-lookup"><span data-stu-id="2c2fe-250">The value of the **Role** attribute in either the Principal or Dependent element of the corresponding ReferentialConstraint element (if used).</span></span>                                                                                                                                                                                                                                             |
| <span data-ttu-id="2c2fe-251">**Multiplizität**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-251">**Multiplicity**</span></span> | <span data-ttu-id="2c2fe-252">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2fe-252">Yes</span></span>         | <span data-ttu-id="2c2fe-253">**1**, **0.. 1**oder **\*** , abhängig von der Anzahl der Zeilen, die am Ende der FOREIGN KEY-Einschränkung liegen können.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-253">**1**, **0..1**, or **\*** depending on the number of rows that can be at the end of the foreign key constraint.</span></span> <br/> <span data-ttu-id="2c2fe-254">der Wert **1** gibt an, dass genau eine Zeile am Ende der Fremdschlüssel Einschränkung vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-254">**1** indicates that exactly one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="2c2fe-255">**0.. 1** gibt an, dass keine oder eine Zeile am Ende der Fremdschlüssel Einschränkung vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-255">**0..1** indicates that zero or one row exists at the foreign key constraint end.</span></span> <br/> <span data-ttu-id="2c2fe-256">**\*** gibt an, dass keine, eine oder mehrere Zeilen am Ende der FOREIGN KEY-Einschränkung vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-256">**\*** indicates that zero, one, or more rows exist at the foreign key constraint end.</span></span> |

> [!NOTE]
> <span data-ttu-id="2c2fe-257">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **End** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-257">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="2c2fe-258">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-258">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2c2fe-259">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-259">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="2c2fe-260">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-260">Example</span></span>

<span data-ttu-id="2c2fe-261">Das folgende Beispiel zeigt ein **Association** -Element, das die FOREIGN KEY-Einschränkung " **FK\_CustomerOrders** " definiert.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-261">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="2c2fe-262">Die **für** jedes **Endelement** angegebenen multiplizitätswerte geben an, dass viele Zeilen in der **Orders** -Tabelle einer Zeile in der **Customers** -Tabelle zugeordnet werden können, aber nur eine Zeile in der **Customers** -Tabelle kann einer Zeile in der **Orders** -Tabelle zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-262">The **Multiplicity** values specified on each **End** element indicate that many rows in the **Orders** table can be associated with a row in the **Customers** table, but only one row in the **Customers** table can be associated with a row in the **Orders** table.</span></span> <span data-ttu-id="2c2fe-263">Außerdem gibt das **OnDelete** -Element an, dass alle Zeilen in der **Orders** -Tabelle, die auf eine bestimmte Zeile in der **Customers** -Tabelle verweisen, gelöscht werden, wenn die Zeile in der **Customers** -Tabelle gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-263">Additionally, the **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="2c2fe-264">Das End-Element als untergeordnetes Objekt des AssociationSet-Elements</span><span class="sxs-lookup"><span data-stu-id="2c2fe-264">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="2c2fe-265">Das **End** -Element (als untergeordnetes Element des **AssociationSet** -Elements) gibt eine Tabelle an einem Ende einer FOREIGN KEY-Einschränkung in der zugrunde liegenden Datenbank an.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-265">The **End** element (as a child of the **AssociationSet** element) specifies a table at one end of a foreign key constraint in the underlying database.</span></span>

<span data-ttu-id="2c2fe-266">Ein **Endelement** kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="2c2fe-266">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2c2fe-267">Dokumentation (0 (null) oder 1)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-267">Documentation (zero or one)</span></span>
-   <span data-ttu-id="2c2fe-268">Anmerkungselemente (kein (null) oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-268">Annotation elements (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="2c2fe-269">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="2c2fe-269">Applicable Attributes</span></span>

<span data-ttu-id="2c2fe-270">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **End** -Element angewendet werden können, wenn es sich um das untergeordnete Element eines **AssociationSet** -Elements handelt.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-270">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="2c2fe-271">Attributname</span><span class="sxs-lookup"><span data-stu-id="2c2fe-271">Attribute Name</span></span> | <span data-ttu-id="2c2fe-272">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="2c2fe-272">Is Required</span></span> | <span data-ttu-id="2c2fe-273">value</span><span class="sxs-lookup"><span data-stu-id="2c2fe-273">Value</span></span>                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2c2fe-274">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-274">**EntitySet**</span></span>  | <span data-ttu-id="2c2fe-275">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2fe-275">Yes</span></span>         | <span data-ttu-id="2c2fe-276">Der Name der SSDL-Entitätenmenge, die sich am Ende der Fremdschlüsseleinschränkung befindet.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-276">The name of the SSDL entity set that is at the end of the foreign key constraint.</span></span>                                      |
| <span data-ttu-id="2c2fe-277">**Rolle**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-277">**Role**</span></span>       | <span data-ttu-id="2c2fe-278">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-278">No</span></span>          | <span data-ttu-id="2c2fe-279">Der Wert eines der **Rollen** Attribute, der in einem **Endelement** des entsprechenden Association-Elements angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-279">The value of one of the **Role** attributes specified on one **End** element of the corresponding Association element.</span></span> |

> [!NOTE]
> <span data-ttu-id="2c2fe-280">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **End** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-280">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="2c2fe-281">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-281">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2c2fe-282">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-282">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

#### <a name="example"></a><span data-ttu-id="2c2fe-283">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-283">Example</span></span>

<span data-ttu-id="2c2fe-284">Das folgende Beispiel zeigt ein **EntityContainer** -Element mit einem **AssociationSet** -Element mit zwei **Endelementen** :</span><span class="sxs-lookup"><span data-stu-id="2c2fe-284">The following example shows an **EntityContainer** element with an **AssociationSet** element with two **End** elements:</span></span>

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

## <a name="entitycontainer-element-ssdl"></a><span data-ttu-id="2c2fe-285">EntityContainer-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-285">EntityContainer Element (SSDL)</span></span>

<span data-ttu-id="2c2fe-286">Ein **EntityContainer** -Element in der Datenspeicher Schema-Definitions Sprache (Store Schema Definition Language, SSDL) beschreibt die Struktur der zugrunde liegenden Datenquelle in einer Entity Framework Anwendung: SSDL-Entitätenmengen (definiert in EntitySet-Elementen) stellen Tabellen in einer Datenbank dar, SSDL-Entitäts Typen (definiert in EntityType-Elementen) stellen Zeilen in einer Tabelle dar, und Zuordnungs Sätze (die in AssociationSet-Elementen definiert sind) stellen Fremdschlüssel Einschränkungen in einer Datenbank dar.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-286">An **EntityContainer** element in store schema definition language (SSDL) describes the structure of the underlying data source in an Entity Framework application: SSDL entity sets (defined in EntitySet elements) represent tables in a database, SSDL entity types (defined in EntityType elements) represent rows in a table, and association sets (defined in AssociationSet elements) represent foreign key constraints in a database.</span></span> <span data-ttu-id="2c2fe-287">Durch das EntityContainerMapping-Element wird einem Speichermodell-Entitätscontainer ein konzeptioneller Modellentitätscontainer zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-287">A storage model entity container maps to a conceptual model entity container through the EntityContainerMapping element.</span></span>

<span data-ttu-id="2c2fe-288">Ein **EntityContainer** -Element kann über 0 (null) oder ein Dokumentations Element verfügen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-288">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="2c2fe-289">Wenn ein **Documentation** -Element vorhanden ist, muss es allen anderen untergeordneten Elementen vorangestellt werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-289">If a **Documentation** element is present, it must precede all other child elements.</span></span>

<span data-ttu-id="2c2fe-290">Ein **EntityContainer** -Element kann über 0 (null) oder mehrere der folgenden untergeordneten Elemente verfügen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="2c2fe-290">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2c2fe-291">EntitySet</span><span class="sxs-lookup"><span data-stu-id="2c2fe-291">EntitySet</span></span>
-   <span data-ttu-id="2c2fe-292">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="2c2fe-292">AssociationSet</span></span>
-   <span data-ttu-id="2c2fe-293">Anmerkungselemente</span><span class="sxs-lookup"><span data-stu-id="2c2fe-293">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2c2fe-294">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="2c2fe-294">Applicable Attributes</span></span>

<span data-ttu-id="2c2fe-295">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **EntityContainer** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-295">The table below describes the attributes that can be applied to the **EntityContainer** element.</span></span>

| <span data-ttu-id="2c2fe-296">Attributname</span><span class="sxs-lookup"><span data-stu-id="2c2fe-296">Attribute Name</span></span> | <span data-ttu-id="2c2fe-297">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="2c2fe-297">Is Required</span></span> | <span data-ttu-id="2c2fe-298">value</span><span class="sxs-lookup"><span data-stu-id="2c2fe-298">Value</span></span>                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| <span data-ttu-id="2c2fe-299">**Name**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-299">**Name**</span></span>       | <span data-ttu-id="2c2fe-300">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2fe-300">Yes</span></span>         | <span data-ttu-id="2c2fe-301">Der Name des Entitätscontainers.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-301">The name of the entity container.</span></span> <span data-ttu-id="2c2fe-302">Dieser Name darf keine Punkte (.) enthalten.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-302">This name cannot contain periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="2c2fe-303">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **EntityContainer** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-303">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="2c2fe-304">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-304">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="2c2fe-305">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-305">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="2c2fe-306">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-306">Example</span></span>

<span data-ttu-id="2c2fe-307">Das folgende Beispiel zeigt ein **EntityContainer** -Element, das zwei Entitätenmengen und einen Zuordnungs Satz definiert.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-307">The following example shows an **EntityContainer** element that defines two entity sets and one association set.</span></span> <span data-ttu-id="2c2fe-308">Beachten Sie, dass die Namen des Entitätstyps und des Zuordnungstyps mit dem Namespace des konzeptionellen Modellnamens qualifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-308">Note that entity type and association type names are qualified by the conceptual model namespace name.</span></span>

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

## <a name="entityset-element-ssdl"></a><span data-ttu-id="2c2fe-309">EntitySet-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-309">EntitySet Element (SSDL)</span></span>

<span data-ttu-id="2c2fe-310">Ein **EntitySet** -Element in der Datenspeicher Schema-Definitions Sprache (SSDL) stellt eine Tabelle oder Sicht in der zugrunde liegenden Datenbank dar.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-310">An **EntitySet** element in store schema definition language (SSDL) represents a table or view in the underlying database.</span></span> <span data-ttu-id="2c2fe-311">Ein EntityType-Element in SSDL stellt eine Zeile in der Tabelle oder der Ansicht dar.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-311">An EntityType element in SSDL represents a row in the table or view.</span></span> <span data-ttu-id="2c2fe-312">Das **EntityType** -Attribut eines **EntitySet** -Elements gibt den spezifischen SSDL-Entitätstyp an, der Zeilen in einer SSDL-Entitätenmenge darstellt.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-312">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="2c2fe-313">Die Zuordnung einer CSDL-Entitätenmenge zu einer SSDL-Entitätenmenge wird in einem EntitySetMapping-Element angegeben.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-313">The mapping between a CSDL entity set and an SSDL entity set is specified in an EntitySetMapping element.</span></span>

<span data-ttu-id="2c2fe-314">Das **EntitySet** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="2c2fe-314">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2c2fe-315">Dokumentation (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-315">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="2c2fe-316">DefiningQuery (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-316">DefiningQuery (zero or one element)</span></span>
-   <span data-ttu-id="2c2fe-317">Anmerkungselemente</span><span class="sxs-lookup"><span data-stu-id="2c2fe-317">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2c2fe-318">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="2c2fe-318">Applicable Attributes</span></span>

<span data-ttu-id="2c2fe-319">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **EntitySet** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-319">The following table describes the attributes that can be applied to the **EntitySet** element.</span></span>

> [!NOTE]
> <span data-ttu-id="2c2fe-320">Einige (hier nicht aufgelistete) Attribute können mit dem **Store** -Alias qualifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-320">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="2c2fe-321">Diese Attribute werden vom Modellaktualisierungs-Assistenten beim Aktualisieren eines Modells verwendet.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-321">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="2c2fe-322">Attributname</span><span class="sxs-lookup"><span data-stu-id="2c2fe-322">Attribute Name</span></span> | <span data-ttu-id="2c2fe-323">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="2c2fe-323">Is Required</span></span> | <span data-ttu-id="2c2fe-324">value</span><span class="sxs-lookup"><span data-stu-id="2c2fe-324">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="2c2fe-325">**Name**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-325">**Name**</span></span>       | <span data-ttu-id="2c2fe-326">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2fe-326">Yes</span></span>         | <span data-ttu-id="2c2fe-327">Der Name der Entitätssammlung.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-327">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="2c2fe-328">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-328">**EntityType**</span></span> | <span data-ttu-id="2c2fe-329">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2fe-329">Yes</span></span>         | <span data-ttu-id="2c2fe-330">Der vollqualifizierte Name des Entitätstyps, für den der Entitätssatz Instanzen enthält.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-330">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |
| <span data-ttu-id="2c2fe-331">**Schema**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-331">**Schema**</span></span>     | <span data-ttu-id="2c2fe-332">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-332">No</span></span>          | <span data-ttu-id="2c2fe-333">Das Datenbankschema.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-333">The database schema.</span></span>                                                                     |
| <span data-ttu-id="2c2fe-334">**Table**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-334">**Table**</span></span>      | <span data-ttu-id="2c2fe-335">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-335">No</span></span>          | <span data-ttu-id="2c2fe-336">Die Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-336">The database table.</span></span>                                                                      |

> [!NOTE]
> <span data-ttu-id="2c2fe-337">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **EntitySet** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-337">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="2c2fe-338">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-338">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="2c2fe-339">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-339">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="2c2fe-340">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-340">Example</span></span>

<span data-ttu-id="2c2fe-341">Das folgende Beispiel zeigt ein **EntityContainer** -Element, das über zwei **EntitySet** -Elemente und ein **AssociationSet** -Element verfügt:</span><span class="sxs-lookup"><span data-stu-id="2c2fe-341">The following example shows an **EntityContainer** element that has two **EntitySet** elements and one **AssociationSet** element:</span></span>

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

## <a name="entitytype-element-ssdl"></a><span data-ttu-id="2c2fe-342">EntityType-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-342">EntityType Element (SSDL)</span></span>

<span data-ttu-id="2c2fe-343">Ein **EntityType** -Element in der Datenspeicher Schema-Definitions Sprache (SSDL) stellt eine Zeile in einer Tabelle oder Sicht der zugrunde liegenden Datenbank dar.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-343">An **EntityType** element in store schema definition language (SSDL) represents a row in a table or view of the underlying database.</span></span> <span data-ttu-id="2c2fe-344">Ein EntitySet-Element in SSDL stellt die Tabelle oder Ansicht dar, in der Zeilen enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-344">An EntitySet element in SSDL represents the table or view in which rows occur.</span></span> <span data-ttu-id="2c2fe-345">Das **EntityType** -Attribut eines **EntitySet** -Elements gibt den spezifischen SSDL-Entitätstyp an, der Zeilen in einer SSDL-Entitätenmenge darstellt.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-345">The **EntityType** attribute of an **EntitySet** element specifies the particular SSDL entity type that represents rows in an SSDL entity set.</span></span> <span data-ttu-id="2c2fe-346">Die Zuordnung eines SSDL-Entitätstyps zu einem CSDL-Entitätstyp wird in einem EntityTypeMapping-Element angegeben.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-346">The mapping between an SSDL entity type and a CSDL entity type is specified in an EntityTypeMapping element.</span></span>

<span data-ttu-id="2c2fe-347">Das **EntityType** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="2c2fe-347">The **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2c2fe-348">Dokumentation (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-348">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="2c2fe-349">Key (kein (null) oder ein Element)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-349">Key (zero or one element)</span></span>
-   <span data-ttu-id="2c2fe-350">Anmerkungselemente</span><span class="sxs-lookup"><span data-stu-id="2c2fe-350">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2c2fe-351">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="2c2fe-351">Applicable Attributes</span></span>

<span data-ttu-id="2c2fe-352">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **EntityType** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-352">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="2c2fe-353">Attributname</span><span class="sxs-lookup"><span data-stu-id="2c2fe-353">Attribute Name</span></span> | <span data-ttu-id="2c2fe-354">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="2c2fe-354">Is Required</span></span> | <span data-ttu-id="2c2fe-355">value</span><span class="sxs-lookup"><span data-stu-id="2c2fe-355">Value</span></span>                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2c2fe-356">**Name**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-356">**Name**</span></span>       | <span data-ttu-id="2c2fe-357">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2fe-357">Yes</span></span>         | <span data-ttu-id="2c2fe-358">Der Name des Entitätstyps.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-358">The name of the entity type.</span></span> <span data-ttu-id="2c2fe-359">Dieser Wert ist normalerweise der gleiche wie der Name der Tabelle, in der der Entitätstyp eine Zeile darstellt.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-359">This value is usually the same as the name of the table in which the entity type represents a row.</span></span> <span data-ttu-id="2c2fe-360">Dieser Wert darf keine Punkte (.) enthalten.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-360">This value can contain no periods (.).</span></span> |

> [!NOTE]
> <span data-ttu-id="2c2fe-361">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **EntityType** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-361">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="2c2fe-362">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-362">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="2c2fe-363">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-363">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="2c2fe-364">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-364">Example</span></span>

<span data-ttu-id="2c2fe-365">Das folgende Beispiel zeigt ein **EntityType** -Element mit zwei Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="2c2fe-365">The following example shows an **EntityType** element with two properties:</span></span>

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

## <a name="function-element-ssdl"></a><span data-ttu-id="2c2fe-366">Function-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-366">Function Element (SSDL)</span></span>

<span data-ttu-id="2c2fe-367">Das **Function** -Element in der Datenspeicher Schema-Definitions Sprache (SSDL) gibt eine gespeicherte Prozedur an, die in der zugrunde liegenden Datenbank vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-367">The **Function** element in store schema definition language (SSDL) specifies a stored procedure that exists in the underlying database.</span></span>

<span data-ttu-id="2c2fe-368">Das **Function** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="2c2fe-368">The **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2c2fe-369">Dokumentation (0 (null) oder 1)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-369">Documentation (zero or one)</span></span>
-   <span data-ttu-id="2c2fe-370">Parameter (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-370">Parameter (zero or more)</span></span>
-   <span data-ttu-id="2c2fe-371">CommandText (0 (null) oder 1)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-371">CommandText (zero or one)</span></span>
-   <span data-ttu-id="2c2fe-372">ReturnType (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-372">ReturnType (zero or more)</span></span>
-   <span data-ttu-id="2c2fe-373">Anmerkungselemente (kein (null) oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-373">Annotation elements (zero or more)</span></span>

<span data-ttu-id="2c2fe-374">Ein Rückgabetyp für eine Funktion muss entweder mit dem **returnType** -Element oder dem **returnType** -Attribut angegeben werden (siehe unten), aber nicht mit beiden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-374">A return type for a function must be specified with either the **ReturnType** element or the **ReturnType** attribute (see below), but not both.</span></span>

<span data-ttu-id="2c2fe-375">Gespeicherte Prozeduren, die im Speichermodell angegeben sind, können in das konzeptionelle Modell einer Anwendung importiert werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-375">Stored procedures that are specified in the storage model can be imported into the conceptual model of an application.</span></span> <span data-ttu-id="2c2fe-376">Weitere Informationen finden Sie unter [Abfragen mit gespeicherten Prozeduren](~/ef6/modeling/designer/stored-procedures/query.md).</span><span class="sxs-lookup"><span data-stu-id="2c2fe-376">For more information, see [Querying with Stored Procedures](~/ef6/modeling/designer/stored-procedures/query.md).</span></span> <span data-ttu-id="2c2fe-377">Das **Function** -Element kann auch verwendet werden, um benutzerdefinierte Funktionen im Speichermodell zu definieren.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-377">The **Function** element can also be used to define custom functions in the storage model.</span></span>  

### <a name="applicable-attributes"></a><span data-ttu-id="2c2fe-378">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="2c2fe-378">Applicable Attributes</span></span>

<span data-ttu-id="2c2fe-379">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **Function** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-379">The following table describes the attributes that can be applied to the **Function** element.</span></span>

> [!NOTE]
> <span data-ttu-id="2c2fe-380">Einige (hier nicht aufgelistete) Attribute können mit dem **Store** -Alias qualifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-380">Some attributes (not listed here) may be qualified with the **store** alias.</span></span> <span data-ttu-id="2c2fe-381">Diese Attribute werden vom Modellaktualisierungs-Assistenten beim Aktualisieren eines Modells verwendet.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-381">These attributes are used by the Update Model Wizard when updating a model.</span></span>

| <span data-ttu-id="2c2fe-382">Attributname</span><span class="sxs-lookup"><span data-stu-id="2c2fe-382">Attribute Name</span></span>             | <span data-ttu-id="2c2fe-383">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="2c2fe-383">Is Required</span></span> | <span data-ttu-id="2c2fe-384">value</span><span class="sxs-lookup"><span data-stu-id="2c2fe-384">Value</span></span>                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2c2fe-385">**Name**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-385">**Name**</span></span>                   | <span data-ttu-id="2c2fe-386">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2fe-386">Yes</span></span>         | <span data-ttu-id="2c2fe-387">Name der gespeicherten Prozedur</span><span class="sxs-lookup"><span data-stu-id="2c2fe-387">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="2c2fe-388">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-388">**ReturnType**</span></span>             | <span data-ttu-id="2c2fe-389">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-389">No</span></span>          | <span data-ttu-id="2c2fe-390">Der Rückgabetyp der gespeicherten Prozedur.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-390">The return type of the stored procedure.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="2c2fe-391">**Aggregat**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-391">**Aggregate**</span></span>              | <span data-ttu-id="2c2fe-392">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-392">No</span></span>          | <span data-ttu-id="2c2fe-393">**True** , wenn die gespeicherte Prozedur einen Aggregatwert zurückgibt. andernfalls **false**.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-393">**True** if the stored procedure returns an aggregate value; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="2c2fe-394">**Builtin**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-394">**BuiltIn**</span></span>                | <span data-ttu-id="2c2fe-395">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-395">No</span></span>          | <span data-ttu-id="2c2fe-396">**True** , wenn es sich bei der Funktion um eine integrierte<sup>1</sup> -Funktion handelt. andernfalls **false**.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-396">**True** if the function is a built-in<sup>1</sup> function; otherwise **False**.</span></span>                                                                                                                                  |
| <span data-ttu-id="2c2fe-397">**Storefunctionname**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-397">**StoreFunctionName**</span></span>      | <span data-ttu-id="2c2fe-398">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-398">No</span></span>          | <span data-ttu-id="2c2fe-399">Name der gespeicherten Prozedur</span><span class="sxs-lookup"><span data-stu-id="2c2fe-399">The name of the stored procedure.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="2c2fe-400">**NiladicFunction**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-400">**NiladicFunction**</span></span>        | <span data-ttu-id="2c2fe-401">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-401">No</span></span>          | <span data-ttu-id="2c2fe-402">**True** , wenn es sich bei der Funktion um eine NILADIC<sup>2</sup> -Funktion handelt. Andernfalls **false** .</span><span class="sxs-lookup"><span data-stu-id="2c2fe-402">**True** if the function is a niladic<sup>2</sup> function; **False** otherwise.</span></span>                                                                                                                                   |
| <span data-ttu-id="2c2fe-403">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-403">**IsComposable**</span></span>           | <span data-ttu-id="2c2fe-404">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-404">No</span></span>          | <span data-ttu-id="2c2fe-405">**True** , wenn die Funktion eine Zusammensetz Bare<sup>3</sup> -Funktion ist. Andernfalls **false** .</span><span class="sxs-lookup"><span data-stu-id="2c2fe-405">**True** if the function is a composable<sup>3</sup> function; **False** otherwise.</span></span>                                                                                                                                |
| <span data-ttu-id="2c2fe-406">**Das**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-406">**ParameterTypeSemantics**</span></span> | <span data-ttu-id="2c2fe-407">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-407">No</span></span>          | <span data-ttu-id="2c2fe-408">Die Enumeration, die die Typsemantik definiert, die zum Auflösen von Funktionsüberladungen verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-408">The enumeration that defines the type semantics used to resolve function overloads.</span></span> <span data-ttu-id="2c2fe-409">Die Enumeration ist im Anbietermanifest für jede Funktionsdefinition definiert.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-409">The enumeration is defined in the provider manifest per function definition.</span></span> <span data-ttu-id="2c2fe-410">Der Standardwert ist " **zuzubemplicitconversion**".</span><span class="sxs-lookup"><span data-stu-id="2c2fe-410">The default value is **AllowImplicitConversion**.</span></span> |
| <span data-ttu-id="2c2fe-411">**Schema**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-411">**Schema**</span></span>                 | <span data-ttu-id="2c2fe-412">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-412">No</span></span>          | <span data-ttu-id="2c2fe-413">Der Name des Schemas, in dem die gespeicherte Prozedur definiert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-413">The name of the schema in which the stored procedure is defined.</span></span>                                                                                                                                                   |

<span data-ttu-id="2c2fe-414"><sup>1</sup> eine integrierte Funktion ist eine Funktion, die in der Datenbank definiert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-414"><sup>1</sup> A built-in function is a function that is defined in the database.</span></span> <span data-ttu-id="2c2fe-415">Informationen zu Funktionen, die im Speichermodell definiert sind, finden Sie unter CommandText-Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="2c2fe-415">For information about functions that are defined in the storage model, see CommandText Element (SSDL).</span></span>

<span data-ttu-id="2c2fe-416"><sup>2</sup> eine NILADIC-Funktion ist eine Funktion, die keine Parameter akzeptiert und, wenn Sie aufgerufen wird, keine Klammern erfordert.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-416"><sup>2</sup> A niladic function is a function that accepts no parameters and, when called, does not require parentheses.</span></span>

<span data-ttu-id="2c2fe-417"><sup>3</sup> zwei Funktionen sind zusammensetzbar, wenn die Ausgabe einer Funktion als Eingabe für die andere Funktion dienen kann.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-417"><sup>3</sup> Two functions are composable if the output of one function can be the input for the other function.</span></span>

> [!NOTE]
> <span data-ttu-id="2c2fe-418">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **Function** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-418">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="2c2fe-419">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-419">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="2c2fe-420">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-420">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="2c2fe-421">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-421">Example</span></span>

<span data-ttu-id="2c2fe-422">Das folgende Beispiel zeigt ein **Function** -Element, das der gespeicherten Prozedur **updateordermenge** entspricht.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-422">The following example shows a **Function** element that corresponds to the **UpdateOrderQuantity** stored procedure.</span></span> <span data-ttu-id="2c2fe-423">Die gespeicherte Prozedur akzeptiert zwei Parameter und gibt keinen Wert zurück.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-423">The stored procedure accepts two parameters and does not return a value.</span></span>

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

## <a name="key-element-ssdl"></a><span data-ttu-id="2c2fe-424">Key-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-424">Key Element (SSDL)</span></span>

<span data-ttu-id="2c2fe-425">Das **Schlüssel** Element in der Datenspeicher Schema-Definitions Sprache (Store Schema Definition Language, SSDL) stellt den Primärschlüssel einer Tabelle in der zugrunde liegenden Datenbank dar.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-425">The **Key** element in store schema definition language (SSDL) represents the primary key of a table in the underlying database.</span></span> <span data-ttu-id="2c2fe-426">**Key** ist ein untergeordnetes Element eines EntityType-Elements, das eine Zeile in einer Tabelle darstellt.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-426">**Key** is a child element of an EntityType element, which represents a row in a table.</span></span> <span data-ttu-id="2c2fe-427">Der Primärschlüssel wird im **Key** -Element definiert, indem auf ein oder mehrere Eigenschafts Elemente verwiesen wird, die für das **EntityType** -Element definiert sind.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-427">The primary key is defined in the **Key** element by referencing one or more Property elements that are defined on the **EntityType** element.</span></span>

<span data-ttu-id="2c2fe-428">Das **Key** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="2c2fe-428">The **Key** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2c2fe-429">PropertyRef (ein oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-429">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="2c2fe-430">Anmerkungselemente</span><span class="sxs-lookup"><span data-stu-id="2c2fe-430">Annotation elements</span></span>

<span data-ttu-id="2c2fe-431">Für das **Key** -Element sind keine Attribute anwendbar.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-431">No attributes are applicable to the **Key** element.</span></span>

### <a name="example"></a><span data-ttu-id="2c2fe-432">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-432">Example</span></span>

<span data-ttu-id="2c2fe-433">Das folgende Beispiel zeigt ein **EntityType** -Element mit einem Schlüssel, der auf eine Eigenschaft verweist:</span><span class="sxs-lookup"><span data-stu-id="2c2fe-433">The following example shows an **EntityType** element with a key that references one property:</span></span>

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

## <a name="ondelete-element-ssdl"></a><span data-ttu-id="2c2fe-434">OnDelete-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-434">OnDelete Element (SSDL)</span></span>

<span data-ttu-id="2c2fe-435">Das **OnDelete** -Element in der Datenspeicher Schema-Definitions Sprache (Store Schema Definition Language, SSDL) spiegelt das Daten Bank Verhalten wider, wenn eine Zeile, die Teil einer FOREIGN KEY-Einschränkung</span><span class="sxs-lookup"><span data-stu-id="2c2fe-435">The **OnDelete** element in store schema definition language (SSDL) reflects the database behavior when a row that participates in a foreign key constraint is deleted.</span></span> <span data-ttu-id="2c2fe-436">Wenn die Aktion auf **Cascade**festgelegt ist, werden Zeilen, die auf eine Zeile verweisen, die gelöscht wird, ebenfalls gelöscht.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-436">If the action is set to **Cascade**, then rows that reference a row that is being deleted will also be deleted.</span></span> <span data-ttu-id="2c2fe-437">Wenn die Aktion auf **None**festgelegt ist, werden Zeilen, die auf eine Zeile verweisen, die gelöscht wird, nicht ebenfalls gelöscht.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-437">If the action is set to **None**, then rows that reference a row that is being deleted are not also deleted.</span></span> <span data-ttu-id="2c2fe-438">Ein **OnDelete** -Element ist ein untergeordnetes Element eines End-Elements.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-438">An **OnDelete** element is a child element of an End element.</span></span>

<span data-ttu-id="2c2fe-439">Ein **OnDelete** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="2c2fe-439">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2c2fe-440">Dokumentation (0 (null) oder 1)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-440">Documentation (zero or one)</span></span>
-   <span data-ttu-id="2c2fe-441">Anmerkungselemente (kein (null) oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-441">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2c2fe-442">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="2c2fe-442">Applicable Attributes</span></span>

<span data-ttu-id="2c2fe-443">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **OnDelete** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-443">The following table describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="2c2fe-444">Attributname</span><span class="sxs-lookup"><span data-stu-id="2c2fe-444">Attribute Name</span></span> | <span data-ttu-id="2c2fe-445">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="2c2fe-445">Is Required</span></span> | <span data-ttu-id="2c2fe-446">value</span><span class="sxs-lookup"><span data-stu-id="2c2fe-446">Value</span></span>                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2c2fe-447">**Aktion**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-447">**Action**</span></span>     | <span data-ttu-id="2c2fe-448">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2fe-448">Yes</span></span>         | <span data-ttu-id="2c2fe-449">**Cascade** oder **None**.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-449">**Cascade** or **None**.</span></span> <span data-ttu-id="2c2fe-450">(Der Wert " **restricted** " ist gültig, hat jedoch das gleiche Verhalten wie " **None**".)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-450">(The value **Restricted** is valid but has the same behavior as **None**.)</span></span> |

> [!NOTE]
> <span data-ttu-id="2c2fe-451">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **OnDelete** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-451">Any number of annotation attributes (custom XML attributes) may be applied to the **OnDelete** element.</span></span> <span data-ttu-id="2c2fe-452">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-452">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="2c2fe-453">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-453">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="2c2fe-454">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-454">Example</span></span>

<span data-ttu-id="2c2fe-455">Das folgende Beispiel zeigt ein **Association** -Element, das die FOREIGN KEY-Einschränkung " **FK\_CustomerOrders** " definiert.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-455">The following example shows an **Association** element that defines the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="2c2fe-456">Das **OnDelete** -Element gibt an, dass alle Zeilen in der **Orders** -Tabelle, die auf eine bestimmte Zeile in der **Customers** -Tabelle verweisen, gelöscht werden, wenn die Zeile in der **Customers** -Tabelle gelöscht wird.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-456">The **OnDelete** element indicates that all rows in the **Orders** table that reference a particular row in the **Customers** table will be deleted if the row in the **Customers** table is deleted.</span></span>

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

## <a name="parameter-element-ssdl"></a><span data-ttu-id="2c2fe-457">Parameter-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-457">Parameter Element (SSDL)</span></span>

<span data-ttu-id="2c2fe-458">Das **Parameter** -Element in der Datenspeicher Schema-Definitions Sprache (SSDL) ist ein untergeordnetes Element des Function-Elements, das Parameter für eine gespeicherte Prozedur in der Datenbank angibt.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-458">The **Parameter** element in store schema definition language (SSDL) is a child of the Function element that specifies parameters for a stored procedure in the database.</span></span>

<span data-ttu-id="2c2fe-459">Das **Parameter** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="2c2fe-459">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2c2fe-460">Dokumentation (0 (null) oder 1)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-460">Documentation (zero or one)</span></span>
-   <span data-ttu-id="2c2fe-461">Anmerkungselemente (kein (null) oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-461">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2c2fe-462">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="2c2fe-462">Applicable Attributes</span></span>

<span data-ttu-id="2c2fe-463">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **Parameter** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-463">The table below describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="2c2fe-464">Attributname</span><span class="sxs-lookup"><span data-stu-id="2c2fe-464">Attribute Name</span></span> | <span data-ttu-id="2c2fe-465">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="2c2fe-465">Is Required</span></span> | <span data-ttu-id="2c2fe-466">value</span><span class="sxs-lookup"><span data-stu-id="2c2fe-466">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2c2fe-467">**Name**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-467">**Name**</span></span>       | <span data-ttu-id="2c2fe-468">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2fe-468">Yes</span></span>         | <span data-ttu-id="2c2fe-469">Der Name des Parameters.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-469">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="2c2fe-470">**Typ**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-470">**Type**</span></span>       | <span data-ttu-id="2c2fe-471">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2fe-471">Yes</span></span>         | <span data-ttu-id="2c2fe-472">Der Parametertyp.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-472">The parameter type.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="2c2fe-473">**Mode**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-473">**Mode**</span></span>       | <span data-ttu-id="2c2fe-474">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-474">No</span></span>          | <span data-ttu-id="2c2fe-475">**In**, out oder **INOUT** , je nachdem, ob der Parameter ein Eingabe-, Ausgabe-oder Eingabe- **/Ausgabeparameter**ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-475">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="2c2fe-476">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-476">**MaxLength**</span></span>  | <span data-ttu-id="2c2fe-477">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-477">No</span></span>          | <span data-ttu-id="2c2fe-478">Die maximale Länge des Parameters.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-478">The maximum length of the parameter.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="2c2fe-479">**Genauigkeit**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-479">**Precision**</span></span>  | <span data-ttu-id="2c2fe-480">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-480">No</span></span>          | <span data-ttu-id="2c2fe-481">Die Genauigkeit des Parameters.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-481">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="2c2fe-482">**Skalieren**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-482">**Scale**</span></span>      | <span data-ttu-id="2c2fe-483">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-483">No</span></span>          | <span data-ttu-id="2c2fe-484">Der Maßstab des Parameters.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-484">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="2c2fe-485">**SRID**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-485">**SRID**</span></span>       | <span data-ttu-id="2c2fe-486">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-486">No</span></span>          | <span data-ttu-id="2c2fe-487">Verweis Bezeichner für räumliche Systeme.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-487">Spatial System Reference Identifier.</span></span> <span data-ttu-id="2c2fe-488">Nur für Parameter räumlicher Typen gültig.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-488">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="2c2fe-489">Weitere Informationen finden Sie unter [SRID](https://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="2c2fe-489">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

> [!NOTE]
> <span data-ttu-id="2c2fe-490">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **Parameter** Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-490">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="2c2fe-491">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-491">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="2c2fe-492">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-492">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="2c2fe-493">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-493">Example</span></span>

<span data-ttu-id="2c2fe-494">Das folgende Beispiel zeigt ein **Function** -Element, das über zwei **Parameter** Elemente verfügt, die Eingabeparameter angeben:</span><span class="sxs-lookup"><span data-stu-id="2c2fe-494">The following example shows a **Function** element that has two **Parameter** elements that specify input parameters:</span></span>

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

## <a name="principal-element-ssdl"></a><span data-ttu-id="2c2fe-495">Prinzipalelement (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-495">Principal Element (SSDL)</span></span>

<span data-ttu-id="2c2fe-496">Das **Principal** -Element in der Speicher Schema-Definitions Sprache (Store Schema Definition Language, SSDL) ist ein untergeordnetes Element des referentialeinschränkung-Elements, das das Prinzipal Ende einer FOREIGN KEY-Einschränkung definiert (auch referenzielle Einschränkung genannt).</span><span class="sxs-lookup"><span data-stu-id="2c2fe-496">The **Principal** element in store schema definition language (SSDL) is a child element to the ReferentialConstraint element that defines the principal end of a foreign key constraint (also called a referential constraint).</span></span> <span data-ttu-id="2c2fe-497">Das **Principal** -Element gibt die Primärschlüssel Spalte (oder Spalten) in einer Tabelle an, auf die von einer anderen Spalte (oder Spalten) verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-497">The **Principal** element specifies the primary key column (or columns) in a table that is referenced by another column (or columns).</span></span> <span data-ttu-id="2c2fe-498">**PropertyRef** -Elemente geben an, auf welche Spalten verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-498">**PropertyRef** elements specify which columns are referenced.</span></span> <span data-ttu-id="2c2fe-499">Das abhängige Element gibt die Spalten an, die auf die Primärschlüssel Spalten verweisen, die im **Principal** -Element angegeben sind.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-499">The Dependent element specifies columns that reference the primary key columns that are specified in the **Principal** element.</span></span>

<span data-ttu-id="2c2fe-500">Das **Principal** -Element kann über die folgenden untergeordneten Elemente verfügen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="2c2fe-500">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="2c2fe-501">PropertyRef (ein oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-501">PropertyRef (one or more)</span></span>
-   <span data-ttu-id="2c2fe-502">Anmerkungselemente (kein (null) oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-502">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2c2fe-503">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="2c2fe-503">Applicable Attributes</span></span>

<span data-ttu-id="2c2fe-504">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **Principal** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-504">The following table describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="2c2fe-505">Attributname</span><span class="sxs-lookup"><span data-stu-id="2c2fe-505">Attribute Name</span></span> | <span data-ttu-id="2c2fe-506">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="2c2fe-506">Is Required</span></span> | <span data-ttu-id="2c2fe-507">value</span><span class="sxs-lookup"><span data-stu-id="2c2fe-507">Value</span></span>                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2c2fe-508">**Rolle**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-508">**Role**</span></span>       | <span data-ttu-id="2c2fe-509">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2fe-509">Yes</span></span>         | <span data-ttu-id="2c2fe-510">Der gleiche Wert wie das **Role** -Attribut (sofern verwendet) des entsprechenden End-Elements. andernfalls der Name der Tabelle, die die Spalte enthält, auf die verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-510">The same value as the **Role** attribute (if used) of the corresponding End element; otherwise, the name of the table that contains the referenced column.</span></span> |

> [!NOTE]
> <span data-ttu-id="2c2fe-511">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **Principal** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-511">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="2c2fe-512">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-512">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2c2fe-513">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-513">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="2c2fe-514">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-514">Example</span></span>

<span data-ttu-id="2c2fe-515">Das folgende Beispiel zeigt ein Association-Element, das ein **referentialeinschränkung** -Element verwendet, um die Spalten anzugeben, die an der **FK-\_CustomerOrders** -Fremdschlüssel Einschränkung teilnehmen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-515">The following example shows an Association element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint.</span></span> <span data-ttu-id="2c2fe-516">Das **Principal** -Element gibt die **CustomerID-** Spalte der **Customer** -Tabelle als Prinzipal Ende der Einschränkung an.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-516">The **Principal** element specifies the **CustomerId** column of the **Customer** table as the principal end of the constraint.</span></span>

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

## <a name="property-element-ssdl"></a><span data-ttu-id="2c2fe-517">Property-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-517">Property Element (SSDL)</span></span>

<span data-ttu-id="2c2fe-518">Das **Property** -Element in der Datenspeicher Schema-Definitions Sprache (SSDL) stellt eine Spalte in einer Tabelle in der zugrunde liegenden Datenbank dar.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-518">The **Property** element in store schema definition language (SSDL) represents a column in a table in the underlying database.</span></span> <span data-ttu-id="2c2fe-519">**Eigenschaften** Elemente sind untergeordnete Elemente von EntityType-Elementen, die Zeilen in einer Tabelle darstellen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-519">**Property** elements are children of EntityType elements, which represent rows in a table.</span></span> <span data-ttu-id="2c2fe-520">Jedes für ein **EntityType** -Element definierte **Eigenschafts** Element stellt eine Spalte dar.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-520">Each **Property** element defined on an **EntityType** element represents a column.</span></span>

<span data-ttu-id="2c2fe-521">Ein **Property** -Element kann keine untergeordneten Elemente aufweisen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-521">A **Property** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2c2fe-522">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="2c2fe-522">Applicable Attributes</span></span>

<span data-ttu-id="2c2fe-523">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **Property** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-523">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="2c2fe-524">Attributname</span><span class="sxs-lookup"><span data-stu-id="2c2fe-524">Attribute Name</span></span>            | <span data-ttu-id="2c2fe-525">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="2c2fe-525">Is Required</span></span> | <span data-ttu-id="2c2fe-526">value</span><span class="sxs-lookup"><span data-stu-id="2c2fe-526">Value</span></span>                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2c2fe-527">**Name**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-527">**Name**</span></span>                  | <span data-ttu-id="2c2fe-528">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2fe-528">Yes</span></span>         | <span data-ttu-id="2c2fe-529">Der Name der zugehörigen Spalte.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-529">The name of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="2c2fe-530">**Typ**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-530">**Type**</span></span>                  | <span data-ttu-id="2c2fe-531">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2fe-531">Yes</span></span>         | <span data-ttu-id="2c2fe-532">Der Typ der zugehörigen Spalte.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-532">The type of the corresponding column.</span></span>                                                                                                                                                                                           |
| <span data-ttu-id="2c2fe-533">**NULL zulassen**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-533">**Nullable**</span></span>              | <span data-ttu-id="2c2fe-534">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-534">No</span></span>          | <span data-ttu-id="2c2fe-535">**True** (Standardwert) oder **false** , abhängig davon, ob die entsprechende Spalte einen NULL-Wert aufweisen kann.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-535">**True** (the default value) or **False** depending on whether the corresponding column can have a null value.</span></span>                                                                                                                  |
| <span data-ttu-id="2c2fe-536">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-536">**DefaultValue**</span></span>          | <span data-ttu-id="2c2fe-537">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-537">No</span></span>          | <span data-ttu-id="2c2fe-538">Der Standardwert der zugehörigen Spalte.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-538">The default value of the corresponding column.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="2c2fe-539">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-539">**MaxLength**</span></span>             | <span data-ttu-id="2c2fe-540">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-540">No</span></span>          | <span data-ttu-id="2c2fe-541">Maximale Länge der zugehörigen Spalte.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-541">The maximum length of the corresponding column.</span></span>                                                                                                                                                                                 |
| <span data-ttu-id="2c2fe-542">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-542">**FixedLength**</span></span>           | <span data-ttu-id="2c2fe-543">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-543">No</span></span>          | <span data-ttu-id="2c2fe-544">**True** oder **false** , abhängig davon, ob der entsprechende Spaltenwert als Zeichenfolge mit fester Länge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-544">**True** or **False** depending on whether the corresponding column value will be stored as a fixed length string.</span></span>                                                                                                              |
| <span data-ttu-id="2c2fe-545">**Genauigkeit**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-545">**Precision**</span></span>             | <span data-ttu-id="2c2fe-546">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-546">No</span></span>          | <span data-ttu-id="2c2fe-547">Die Genauigkeit der zugehörigen Spalte.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-547">The precision of the corresponding column.</span></span>                                                                                                                                                                                      |
| <span data-ttu-id="2c2fe-548">**Skalieren**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-548">**Scale**</span></span>                 | <span data-ttu-id="2c2fe-549">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-549">No</span></span>          | <span data-ttu-id="2c2fe-550">Die Dezimalstellenanzahl der zugehörigen Spalte.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-550">The scale of the corresponding column.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="2c2fe-551">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-551">**Unicode**</span></span>               | <span data-ttu-id="2c2fe-552">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-552">No</span></span>          | <span data-ttu-id="2c2fe-553">**True** oder **false** , abhängig davon, ob der entsprechende Spaltenwert als Unicode-Zeichenfolge gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-553">**True** or **False** depending on whether the corresponding column value will be stored as a Unicode string.</span></span>                                                                                                                   |
| <span data-ttu-id="2c2fe-554">**Sortierung**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-554">**Collation**</span></span>             | <span data-ttu-id="2c2fe-555">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-555">No</span></span>          | <span data-ttu-id="2c2fe-556">Eine Zeichenfolge, die angibt, welche Sortierreihenfolge in der Datenquelle verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-556">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="2c2fe-557">**SRID**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-557">**SRID**</span></span>                  | <span data-ttu-id="2c2fe-558">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-558">No</span></span>          | <span data-ttu-id="2c2fe-559">Verweis Bezeichner für räumliche Systeme.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-559">Spatial System Reference Identifier.</span></span> <span data-ttu-id="2c2fe-560">Nur für Eigenschaften räumlicher Typen gültig.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-560">Valid only for properties of spatial types.</span></span> <span data-ttu-id="2c2fe-561">Weitere Informationen finden Sie unter [SRID](https://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="2c2fe-561">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="2c2fe-562">**StoreGeneratedPattern**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-562">**StoreGeneratedPattern**</span></span> | <span data-ttu-id="2c2fe-563">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-563">No</span></span>          | <span data-ttu-id="2c2fe-564">**None**, **Identity** (wenn der entsprechende Spaltenwert eine in der Datenbank generierte Identität ist) oder **berechnet** (wenn der entsprechende Spaltenwert in der Datenbank berechnet wird).</span><span class="sxs-lookup"><span data-stu-id="2c2fe-564">**None**, **Identity** (if the corresponding column value is an identity that is generated in the database), or **Computed** (if the corresponding column value is computed in the database).</span></span> <span data-ttu-id="2c2fe-565">Ungültig für RowType-Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-565">Not Valid for RowType properties.</span></span> |

> [!NOTE]
> <span data-ttu-id="2c2fe-566">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **Property** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-566">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="2c2fe-567">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-567">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="2c2fe-568">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-568">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="2c2fe-569">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-569">Example</span></span>

<span data-ttu-id="2c2fe-570">Das folgende Beispiel zeigt ein **EntityType** -Element mit zwei untergeordneten **Eigenschaften** Elementen:</span><span class="sxs-lookup"><span data-stu-id="2c2fe-570">The following example shows an **EntityType** element with two child **Property** elements:</span></span>

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

## <a name="propertyref-element-ssdl"></a><span data-ttu-id="2c2fe-571">PropertyRef-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-571">PropertyRef Element (SSDL)</span></span>

<span data-ttu-id="2c2fe-572">Das **PropertyRef** -Element in der Speicher Schema-Definitions Sprache (Store Schema Definition Language, SSDL) verweist auf eine für ein EntityType-Element definierte Eigenschaft, um anzugeben, dass die-Eigenschaft eine der folgenden Rollen ausführt:</span><span class="sxs-lookup"><span data-stu-id="2c2fe-572">The **PropertyRef** element in store schema definition language (SSDL) references a property defined on an EntityType element to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="2c2fe-573">Ist Teil des Primärschlüssels der Tabelle, die der **EntityType** darstellt.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-573">Be part of the primary key of the table that the **EntityType** represents.</span></span> <span data-ttu-id="2c2fe-574">Ein oder mehrere **PropertyRef** -Elemente können verwendet werden, um einen Primärschlüssel zu definieren.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-574">One or more **PropertyRef** elements can be used to define a primary key.</span></span> <span data-ttu-id="2c2fe-575">Weitere Informationen finden Sie unter Key-Element.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-575">For more information, see Key element.</span></span>
-   <span data-ttu-id="2c2fe-576">Sie ist das abhängige Ende oder das Prinzipalende einer referenziellen Einschränkung.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-576">Be the dependent or principal end of a referential constraint.</span></span> <span data-ttu-id="2c2fe-577">Weitere Informationen finden Sie unter ReferentialConstraint-Element.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-577">For more information, see ReferentialConstraint element.</span></span>

<span data-ttu-id="2c2fe-578">Das **PropertyRef** -Element kann nur die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="2c2fe-578">The **PropertyRef** element can only have the following child elements:</span></span>

-   <span data-ttu-id="2c2fe-579">Dokumentation (0 (null) oder 1)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-579">Documentation (zero or one)</span></span>
-   <span data-ttu-id="2c2fe-580">Anmerkungselemente</span><span class="sxs-lookup"><span data-stu-id="2c2fe-580">Annotation elements</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2c2fe-581">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="2c2fe-581">Applicable Attributes</span></span>

<span data-ttu-id="2c2fe-582">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **PropertyRef** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-582">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="2c2fe-583">Attributname</span><span class="sxs-lookup"><span data-stu-id="2c2fe-583">Attribute Name</span></span> | <span data-ttu-id="2c2fe-584">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="2c2fe-584">Is Required</span></span> | <span data-ttu-id="2c2fe-585">value</span><span class="sxs-lookup"><span data-stu-id="2c2fe-585">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="2c2fe-586">**Name**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-586">**Name**</span></span>       | <span data-ttu-id="2c2fe-587">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2fe-587">Yes</span></span>         | <span data-ttu-id="2c2fe-588">Der Name der referenzierten Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-588">The name of the referenced property.</span></span> |

> [!NOTE]
> <span data-ttu-id="2c2fe-589">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **PropertyRef** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-589">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="2c2fe-590">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-590">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="2c2fe-591">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-591">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="2c2fe-592">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-592">Example</span></span>

<span data-ttu-id="2c2fe-593">Das folgende Beispiel zeigt ein **PropertyRef** -Element, das verwendet wird, um einen Primärschlüssel zu definieren, indem auf eine Eigenschaft verwiesen wird, die für ein **EntityType** -Element definiert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-593">The following example shows a **PropertyRef** element used to define a primary key by referencing a property that is defined on an **EntityType** element.</span></span>

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

## <a name="referentialconstraint-element-ssdl"></a><span data-ttu-id="2c2fe-594">ReferentialConstraint-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-594">ReferentialConstraint Element (SSDL)</span></span>

<span data-ttu-id="2c2fe-595">Das **referentialeinschränkungs** -Element in der Datenspeicher Schema-Definitions Sprache (SSDL) stellt eine FOREIGN KEY-Einschränkung (auch referenzielle Integritäts Einschränkung genannt) in der zugrunde liegenden Datenbank dar.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-595">The **ReferentialConstraint** element in store schema definition language (SSDL) represents a foreign key constraint (also called a referential integrity constraint) in the underlying database.</span></span> <span data-ttu-id="2c2fe-596">Das Prinzipalende und das abhängige Ende der Einschränkung werden durch das Principal-Element bzw. das Dependent-Element angegeben.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-596">The principal and dependent ends of the constraint are specified by the Principal and Dependent child elements, respectively.</span></span> <span data-ttu-id="2c2fe-597">Auf Spalten, die am Prinzipalende und am abhängigen Enden beteiligt sind, wird mit PropertyRef-Elementen verwiesen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-597">Columns that participate in the principal and dependent ends are referenced with PropertyRef elements.</span></span>

<span data-ttu-id="2c2fe-598">Das **referentialeinschränkung** -Element ist ein optionales untergeordnetes Element des Association-Elements.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-598">The **ReferentialConstraint** element is an optional child element of the Association element.</span></span> <span data-ttu-id="2c2fe-599">Wenn ein **referentialeinschränkungs** -Element nicht verwendet wird, um die FOREIGN KEY-Einschränkung zuzuordnen, die im **Association** -Element angegeben ist, muss hierfür ein AssociationSetMapping-Element verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-599">If a **ReferentialConstraint** element is not used to map the foreign key constraint that is specified in the **Association** element, an AssociationSetMapping element must be used to do this.</span></span>

<span data-ttu-id="2c2fe-600">Das **referentialeinschränkung** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="2c2fe-600">The **ReferentialConstraint** element can have the following child elements:</span></span>

-   <span data-ttu-id="2c2fe-601">Dokumentation (0 (null) oder 1)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-601">Documentation (zero or one)</span></span>
-   <span data-ttu-id="2c2fe-602">Principal (genau ein Element)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-602">Principal (exactly one)</span></span>
-   <span data-ttu-id="2c2fe-603">Abhängig (genau 1)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-603">Dependent (exactly one)</span></span>
-   <span data-ttu-id="2c2fe-604">Anmerkungselemente (kein (null) oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-604">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2c2fe-605">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="2c2fe-605">Applicable Attributes</span></span>

<span data-ttu-id="2c2fe-606">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **referentialeinschränkung** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-606">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferentialConstraint** element.</span></span> <span data-ttu-id="2c2fe-607">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-607">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="2c2fe-608">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-608">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="2c2fe-609">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-609">Example</span></span>

<span data-ttu-id="2c2fe-610">Das folgende Beispiel zeigt ein **Association** -Element, das ein **referentialeinschränkung** -Element verwendet, um die Spalten anzugeben, die an der **FK-\_CustomerOrders** -Foreign Key-Einschränkung beteiligt sind:</span><span class="sxs-lookup"><span data-stu-id="2c2fe-610">The following example shows an **Association** element that uses a **ReferentialConstraint** element to specify the columns that participate in the **FK\_CustomerOrders** foreign key constraint:</span></span>

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

## <a name="returntype-element-ssdl"></a><span data-ttu-id="2c2fe-611">ReturnType-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-611">ReturnType Element (SSDL)</span></span>

<span data-ttu-id="2c2fe-612">Das **returnType** -Element in der Speicher Schema-Definitions Sprache (SSDL) gibt den Rückgabetyp für eine Funktion an, die in einem **Function** -Element definiert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-612">The **ReturnType** element in store schema definition language (SSDL) specifies the return type for a function that is defined in a **Function** element.</span></span> <span data-ttu-id="2c2fe-613">Ein Funktions Rückgabetyp kann auch mit einem **returnType** -Attribut angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-613">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="2c2fe-614">Der Rückgabetyp einer Funktion wird mit dem **Type** -Attribut oder dem **returnType** -Element angegeben.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-614">The return type of a function is specified with the **Type** attribute or the **ReturnType** element.</span></span>

<span data-ttu-id="2c2fe-615">Das **returnType** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="2c2fe-615">The **ReturnType** element can have the following child elements:</span></span>

- <span data-ttu-id="2c2fe-616">CollectionType (eins)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-616">CollectionType (one)</span></span>  

> [!NOTE]
> <span data-ttu-id="2c2fe-617">Eine beliebige Anzahl von Anmerkung-Attributen (benutzerdefinierte XML-Attribute) kann auf das **returnType** -Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-617">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** element.</span></span> <span data-ttu-id="2c2fe-618">Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-618">However, custom attributes may not belong to any XML namespace that is reserved for SSDL.</span></span> <span data-ttu-id="2c2fe-619">Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="2c2fe-620">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-620">Example</span></span>

<span data-ttu-id="2c2fe-621">Im folgenden Beispiel wird eine **Funktion** verwendet, die eine Auflistung von Zeilen zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-621">The following example uses a **Function** that returns a collection of rows.</span></span>

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


## <a name="rowtype-element-ssdl"></a><span data-ttu-id="2c2fe-622">RowType-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-622">RowType Element (SSDL)</span></span>

<span data-ttu-id="2c2fe-623">Ein **RowType** -Element in der Speicher Schema-Definitions Sprache (Store Schema Definition Language, SSDL) definiert eine unbenannte Struktur als Rückgabetyp für eine im Speicher definierte Funktion.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-623">A **RowType** element in store schema definition language (SSDL) defines an unnamed structure as a return type for a function defined in the store.</span></span>

<span data-ttu-id="2c2fe-624">Ein **RowType** -Element ist das untergeordnete Element des **CollectionType** -Elements:</span><span class="sxs-lookup"><span data-stu-id="2c2fe-624">A **RowType** element is the child element of **CollectionType** element:</span></span>

<span data-ttu-id="2c2fe-625">Ein **RowType** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="2c2fe-625">A **RowType** element can have the following child elements:</span></span>

- <span data-ttu-id="2c2fe-626">Eigenschaft (ein oder mehrere Elemente)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-626">Property (one or more)</span></span>  

### <a name="example"></a><span data-ttu-id="2c2fe-627">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-627">Example</span></span>

<span data-ttu-id="2c2fe-628">Das folgende Beispiel zeigt eine Store-Funktion, die ein **CollectionType** -Element verwendet, um anzugeben, dass die Funktion eine Auflistung von Zeilen zurückgibt (wie im **RowType** -Element angegeben).</span><span class="sxs-lookup"><span data-stu-id="2c2fe-628">The following example shows a store function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>


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

## <a name="schema-element-ssdl"></a><span data-ttu-id="2c2fe-629">Schema-Element (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-629">Schema Element (SSDL)</span></span>

<span data-ttu-id="2c2fe-630">Das **Schema** -Element in der Speicher Schema-Definitions Sprache (Store Schema Definition Language, SSDL) ist das Stamm Element einer Speichermodell Definition.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-630">The **Schema** element in store schema definition language (SSDL) is the root element of a storage model definition.</span></span> <span data-ttu-id="2c2fe-631">Es enthält Definitionen für die Objekte, Funktionen und Container, die zusammen ein Speichermodell bilden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-631">It contains definitions for the objects, functions, and containers that make up a storage model.</span></span>

<span data-ttu-id="2c2fe-632">Das **Schema** Element kann NULL oder mehr der folgenden untergeordneten Elemente enthalten:</span><span class="sxs-lookup"><span data-stu-id="2c2fe-632">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="2c2fe-633">Zuordnung</span><span class="sxs-lookup"><span data-stu-id="2c2fe-633">Association</span></span>
-   <span data-ttu-id="2c2fe-634">EntityType</span><span class="sxs-lookup"><span data-stu-id="2c2fe-634">EntityType</span></span>
-   <span data-ttu-id="2c2fe-635">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="2c2fe-635">EntityContainer</span></span>
-   <span data-ttu-id="2c2fe-636">Funktion</span><span class="sxs-lookup"><span data-stu-id="2c2fe-636">Function</span></span>

<span data-ttu-id="2c2fe-637">Das **Schema** -Element verwendet das **Namespace** -Attribut, um den Namespace für den Entitätstyp und die Zuordnungs Objekte in einem Speichermodell zu definieren.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-637">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type and association objects in a storage model.</span></span> <span data-ttu-id="2c2fe-638">Innerhalb eines Namespace müssen alle Objekte eine eindeutige Bezeichnung aufweisen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-638">Within a namespace, no two objects can have the same name.</span></span>

<span data-ttu-id="2c2fe-639">Ein Speichermodell-Namespace unterscheidet sich vom XML-Namespace des **Schema** -Elements.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-639">A storage model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="2c2fe-640">Ein Speichermodell-Namespace (wie durch das **Namespace** -Attribut definiert) ist ein logischer Container für Entitäts Typen und Zuordnungs Typen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-640">A storage model namespace (as defined by the **Namespace** attribute) is a logical container for entity types and association types.</span></span> <span data-ttu-id="2c2fe-641">Der XML-Namespace (angegeben durch das **xmlns** -Attribut) eines **Schema** -Elements ist der Standard Namespace für untergeordnete Elemente und Attribute des **Schema** -Elements.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-641">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="2c2fe-642">XML-Namespaces der Form https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (wobei yyyy und mm jeweils ein Jahr und einen Monat darstellen) sind für SSDL reserviert.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-642">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (where YYYY and MM represent a year and month respectively) are reserved for SSDL.</span></span> <span data-ttu-id="2c2fe-643">Benutzerdefinierte Elemente und Attribute können nicht in Namespaces mit diesem Format vorhanden sein.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-643">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="2c2fe-644">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="2c2fe-644">Applicable Attributes</span></span>

<span data-ttu-id="2c2fe-645">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **Schema** Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-645">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="2c2fe-646">Attributname</span><span class="sxs-lookup"><span data-stu-id="2c2fe-646">Attribute Name</span></span>            | <span data-ttu-id="2c2fe-647">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="2c2fe-647">Is Required</span></span> | <span data-ttu-id="2c2fe-648">value</span><span class="sxs-lookup"><span data-stu-id="2c2fe-648">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2c2fe-649">**Namespace**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-649">**Namespace**</span></span>             | <span data-ttu-id="2c2fe-650">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2fe-650">Yes</span></span>         | <span data-ttu-id="2c2fe-651">Der Namespace für das Speichermodell.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-651">The namespace of the storage model.</span></span> <span data-ttu-id="2c2fe-652">Der Wert des **Namespace** -Attributs wird verwendet, um den voll qualifizierten Namen eines Typs zu bilden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-652">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="2c2fe-653">Wenn sich beispielsweise ein **EntityType** mit dem Namen *Customer* im examplemodel. Store-Namespace befindet, ist der voll qualifizierte Name von **EntityType** examplemodel. Store. Customer.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-653">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace, then the fully qualified name of the **EntityType** is ExampleModel.Store.Customer.</span></span> <br/> <span data-ttu-id="2c2fe-654">Die folgenden Zeichen folgen können nicht als Wert für das **Namespace** -Attribut verwendet werden: **System**, **transient**oder **EDM**.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-654">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="2c2fe-655">Der Wert für das **Namespace** -Attribut darf nicht mit dem Wert für das **Namespace** -Attribut im CSDL-Schema Element identisch sein.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-655">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the CSDL Schema element.</span></span> |
| <span data-ttu-id="2c2fe-656">**Alias**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-656">**Alias**</span></span>                 | <span data-ttu-id="2c2fe-657">Nein</span><span class="sxs-lookup"><span data-stu-id="2c2fe-657">No</span></span>          | <span data-ttu-id="2c2fe-658">Ein anstelle der Namespacebezeichnung verwendeter Bezeichner.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-658">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="2c2fe-659">Wenn sich z. b. ein **EntityType** mit dem Namen *Customer* im examplemodel. Store-Namespace und der Wert des **Alias** -Attributs *storagemodel*befindet, können Sie storagemodel. Customer als voll qualifizierten Namen des **EntityType verwenden.**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-659">For example, if an **EntityType** named *Customer* is in the ExampleModel.Store namespace and the value of the **Alias** attribute is *StorageModel*, then you can use StorageModel.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="2c2fe-660">**Anbieter**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-660">**Provider**</span></span>              | <span data-ttu-id="2c2fe-661">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2fe-661">Yes</span></span>         | <span data-ttu-id="2c2fe-662">Der Datenanbieter.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-662">The data provider.</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span data-ttu-id="2c2fe-663">**ProviderManifestToken**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-663">**ProviderManifestToken**</span></span> | <span data-ttu-id="2c2fe-664">Ja</span><span class="sxs-lookup"><span data-stu-id="2c2fe-664">Yes</span></span>         | <span data-ttu-id="2c2fe-665">Ein Token, das dem Anbieter angibt, welches Anbietermanifest zurückgegeben werden soll.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-665">A token that indicates to the provider which provider manifest to return.</span></span> <span data-ttu-id="2c2fe-666">Für das Token ist kein Format definiert.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-666">No format for the token is defined.</span></span> <span data-ttu-id="2c2fe-667">Die für das Token möglichen Werte werden vom Anbieter definiert.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-667">Values for the token are defined by the provider.</span></span> <span data-ttu-id="2c2fe-668">Informationen zu SQL Server-Anbieter Manifest-Token finden Sie unter SqlClient for Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-668">For information about SQL Server provider manifest tokens, see SqlClient for Entity Framework.</span></span>                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a><span data-ttu-id="2c2fe-669">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-669">Example</span></span>

<span data-ttu-id="2c2fe-670">Das folgende Beispiel zeigt ein **Schema** -Element, das ein **EntityContainer** -Element, zwei **EntityType** -Elemente und ein **Association** -Element enthält.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-670">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema Namespace="ExampleModel.Store"
       Alias="Self" Provider="System.Data.SqlClient"
       ProviderManifestToken="2008"
       xmlns="https://schemas.microsoft.com/ado/2009/11/edm/ssdl">
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

## <a name="annotation-attributes"></a><span data-ttu-id="2c2fe-671">Anmerkungsattribute</span><span class="sxs-lookup"><span data-stu-id="2c2fe-671">Annotation Attributes</span></span>

<span data-ttu-id="2c2fe-672">Anmerkungsattribute sind in der Datenspeicherschema-Definitionssprache (Store Schema Definition Language, SSDL) benutzerdefinierte XML-Attribute im Speichermodell, die zusätzliche Metadaten zu den Elementen im Speichermodell bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-672">Annotation attributes in store schema definition language (SSDL) are custom XML attributes in the storage model that provide extra metadata about the elements in the storage model.</span></span> <span data-ttu-id="2c2fe-673">Neben dem Vorhandensein einer gültigen XML-Struktur gelten für Anmerkungsattribute folgende Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="2c2fe-673">In addition to having valid XML structure, the following constraints apply to annotation attributes:</span></span>

-   <span data-ttu-id="2c2fe-674">Anmerkungsattribute dürfen sich in keinem XML-Namespace befinden, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-674">Annotation attributes must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="2c2fe-675">Die vollqualifizierten Namen zweier Anmerkungsattribute dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-675">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="2c2fe-676">Für ein angegebenes SSDL-Element kann mehr als ein Anmerkungsattribut übernommen werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-676">More than one annotation attribute may be applied to a given SSDL element.</span></span> <span data-ttu-id="2c2fe-677">Auf Metadaten, die in Anmerkung-Elementen enthalten sind, kann zur Laufzeit mithilfe von Klassen im System. Data. Metadata. Edm-Namespace zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-677">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="2c2fe-678">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-678">Example</span></span>

<span data-ttu-id="2c2fe-679">Das folgende Beispiel zeigt ein EntityType-Element, das über ein Anmerkung-Attribut verfügt, das auf die **OrderID** -Eigenschaft angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-679">The following example shows an EntityType element that has an annotation attribute applied to the **OrderId** property.</span></span> <span data-ttu-id="2c2fe-680">Das Beispiel zeigt auch ein Anmerkung-Element, das dem **EntityType** -Element hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-680">The example also show an annotation element added to the **EntityType** element.</span></span>

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

## <a name="annotation-elements-ssdl"></a><span data-ttu-id="2c2fe-681">Anmerkungelemente (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-681">Annotation Elements (SSDL)</span></span>

<span data-ttu-id="2c2fe-682">Anmerkungselemente sind in der Datenspeicherschema-Definitionssprache (Store Schema Definition Language, SSDL) benutzerdefinierte XML-Elemente im Speichermodell, die zusätzliche Metadaten zum Speichermodell bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-682">Annotation elements in store schema definition language (SSDL) are custom XML elements in the storage model that provide extra metadata about the storage model.</span></span> <span data-ttu-id="2c2fe-683">Neben dem Vorhandensein einer gültigen XML-Struktur gelten für Anmerkungselemente folgende Einschränkungen:</span><span class="sxs-lookup"><span data-stu-id="2c2fe-683">In addition to having valid XML structure, the following constraints apply to annotation elements:</span></span>

-   <span data-ttu-id="2c2fe-684">Anmerkungselemente dürfen sich in keinem XML-Namespace befinden, der für SSDL reserviert ist.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-684">Annotation elements must not be in any XML namespace that is reserved for SSDL.</span></span>
-   <span data-ttu-id="2c2fe-685">Die vollqualifizierten Namen zweier Anmerkungselemente dürfen nicht übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-685">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="2c2fe-686">Anmerkungselemente müssen nach allen anderen untergeordneten Elementen eines angegebenen SSDL-Elements angeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-686">Annotation elements must appear after all other child elements of a given SSDL element.</span></span>

<span data-ttu-id="2c2fe-687">Mehrere Anmerkungselemente können untergeordnete Elemente eines angegebenen SSDL-Elements sein.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-687">More than one annotation element may be a child of a given SSDL element.</span></span> <span data-ttu-id="2c2fe-688">Ab Version 4 von .NET Framework können mithilfe von Klassen im Namespace "System. Data. Metadata. Edm" zur Laufzeit auf Metadaten zugegriffen werden, die in Anmerkung-Elementen enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-688">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="2c2fe-689">Beispiel</span><span class="sxs-lookup"><span data-stu-id="2c2fe-689">Example</span></span>

<span data-ttu-id="2c2fe-690">Das folgende Beispiel zeigt ein EntityType-Element, das über ein Anmerkung-Element (**customelement**) verfügt.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-690">The following example shows an EntityType element that has an annotation element (**CustomElement**).</span></span> <span data-ttu-id="2c2fe-691">Das Beispiel zeigt auch ein Anmerkung-Attribut, das auf die **OrderID** -Eigenschaft angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-691">The example also shows an annotation attribute applied to the **OrderId** property.</span></span>

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

## <a name="facets-ssdl"></a><span data-ttu-id="2c2fe-692">Facets (SSDL)</span><span class="sxs-lookup"><span data-stu-id="2c2fe-692">Facets (SSDL)</span></span>

<span data-ttu-id="2c2fe-693">Facets stellen in der Datenspeicherschema-Definitionssprache (Store Schema Definition Language, SSDL) Einschränkungen für Spaltentypen dar, die in Eigenschaftenelementen angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-693">Facets in store schema definition language (SSDL) represent constraints on column types that are specified in Property elements.</span></span> <span data-ttu-id="2c2fe-694">Facetten werden als XML-Attribute für **Eigenschaften** Elemente implementiert.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-694">Facets are implemented as XML attributes on **Property** elements.</span></span>

<span data-ttu-id="2c2fe-695">In der folgenden Tabelle werden die in SSDL unterstützten Facets beschrieben:</span><span class="sxs-lookup"><span data-stu-id="2c2fe-695">The following table describes the facets that are supported in SSDL:</span></span>

| <span data-ttu-id="2c2fe-696">Facet</span><span class="sxs-lookup"><span data-stu-id="2c2fe-696">Facet</span></span>           | <span data-ttu-id="2c2fe-697">BESCHREIBUNG</span><span class="sxs-lookup"><span data-stu-id="2c2fe-697">Description</span></span>                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="2c2fe-698">**Sortierung**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-698">**Collation**</span></span>   | <span data-ttu-id="2c2fe-699">Gibt die bei Vergleich- und Sortiervorgängen zu verwendende Sortierreihenfolge für die Werte der Eigenschaft an.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-699">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                             |
| <span data-ttu-id="2c2fe-700">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-700">**FixedLength**</span></span> | <span data-ttu-id="2c2fe-701">Gibt an, ob sich die Länge des Spaltenwerts ändern kann.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-701">Specifies whether the length of the column value can vary.</span></span>                                                                                                                                                                                                  |
| <span data-ttu-id="2c2fe-702">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-702">**MaxLength**</span></span>   | <span data-ttu-id="2c2fe-703">Gibt die maximale Länge des Spaltenwerts an.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-703">Specifies the maximum length of the column value.</span></span>                                                                                                                                                                                                           |
| <span data-ttu-id="2c2fe-704">**Genauigkeit**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-704">**Precision**</span></span>   | <span data-ttu-id="2c2fe-705">Gibt bei Eigenschaften vom Typ **Decimal**die Anzahl der Ziffern an, die ein Eigenschafts Wert aufweisen kann.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-705">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="2c2fe-706">Bei Eigenschaften vom Typ **time**, **DateTime**und **DateTimeOffset**wird die Anzahl von Ziffern für die Sekundenbruchteile des Spaltenwerts angegeben.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-706">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the column value.</span></span> |
| <span data-ttu-id="2c2fe-707">**Skalieren**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-707">**Scale**</span></span>       | <span data-ttu-id="2c2fe-708">Gibt die Anzahl der Dezimalstellen für den Spaltenwert an.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-708">Specifies the number of digits to the right of the decimal point for the column value.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="2c2fe-709">**Unicode**</span><span class="sxs-lookup"><span data-stu-id="2c2fe-709">**Unicode**</span></span>     | <span data-ttu-id="2c2fe-710">Gibt an, ob der Spaltenwert als Unicode gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="2c2fe-710">Indicates whether the column value is stored as Unicode.</span></span>                                                                                                                                                                                                    |
