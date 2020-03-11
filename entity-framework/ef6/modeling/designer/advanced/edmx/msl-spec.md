---
title: MSL-Spezifikation-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 8990d1373ea2121ce11337a43dbcdf3b9e1532bd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415478"
---
# <a name="msl-specification"></a><span data-ttu-id="59722-102">MSL-Spezifikation</span><span class="sxs-lookup"><span data-stu-id="59722-102">MSL Specification</span></span>
<span data-ttu-id="59722-103">Bei der Mapping-Spezifikationssprache (MSL) handelt es sich um eine XML-basierte Sprache, die die Zuordnung zwischen dem konzeptionellen Modell und dem Speichermodell einer Entity Framework Anwendung beschreibt.</span><span class="sxs-lookup"><span data-stu-id="59722-103">Mapping specification language (MSL) is an XML-based language that describes the mapping between the conceptual model and storage model of an Entity Framework application.</span></span>

<span data-ttu-id="59722-104">In einer Entity Framework Anwendung werden die Mapping-Metadaten zur Buildzeit aus einer MSL-Datei (geschrieben in MSL) geladen.</span><span class="sxs-lookup"><span data-stu-id="59722-104">In an Entity Framework application, mapping metadata is loaded from an .msl file (written in MSL) at build time.</span></span> <span data-ttu-id="59722-105">Entity Framework verwendet zur Laufzeit Mapping-Metadaten, um Abfragen für das konzeptionelle Modell in Speicher spezifische Befehle zu übersetzen.</span><span class="sxs-lookup"><span data-stu-id="59722-105">Entity Framework uses mapping metadata at runtime to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="59722-106">Der Entity Framework Designer (EF-Designer) speichert Zuordnungsinformationen in einer EDMX-Datei zur Entwurfszeit.</span><span class="sxs-lookup"><span data-stu-id="59722-106">The Entity Framework Designer (EF Designer) stores mapping information in an .edmx file at design time.</span></span> <span data-ttu-id="59722-107">Zur Buildzeit verwendet die Entity Designer Informationen in einer EDMX-Datei, um die MSL-Datei zu erstellen, die zur Laufzeit von Entity Framework benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="59722-107">At build time, the Entity Designer uses information in an .edmx file to create the .msl file that is needed by Entity Framework at runtime</span></span>

<span data-ttu-id="59722-108">Namen aller konzeptionellen oder Speichermodelltypen, auf denen in MSL verwiesen wird, müssen mit dem jeweiligen Namespacenamen qualifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="59722-108">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="59722-109">Weitere Informationen zum Namespace Namen des konzeptionellen Modells finden Sie unter [CSDL-Spezifikation](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="59722-109">For information about the conceptual model namespace name, see [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span> <span data-ttu-id="59722-110">Weitere Informationen zum Namespace Namen des Speicher Modells finden Sie unter [SSDL-Spezifikation](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="59722-110">For information about the storage model namespace name, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="59722-111">MSL-Versionen unterscheiden sich von XML-Namespaces.</span><span class="sxs-lookup"><span data-stu-id="59722-111">Versions of MSL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="59722-112">MSL-Version</span><span class="sxs-lookup"><span data-stu-id="59722-112">MSL Version</span></span> | <span data-ttu-id="59722-113">XML-Namespace</span><span class="sxs-lookup"><span data-stu-id="59722-113">XML Namespace</span></span>                                        |
|:------------|:-----------------------------------------------------|
| <span data-ttu-id="59722-114">MSL v1</span><span class="sxs-lookup"><span data-stu-id="59722-114">MSL v1</span></span>      | <span data-ttu-id="59722-115">urn: Schemas-Microsoft-com: Windows: Storage: Mapping: CS</span><span class="sxs-lookup"><span data-stu-id="59722-115">urn:schemas-microsoft-com:windows:storage:mapping:CS</span></span> |
| <span data-ttu-id="59722-116">MSL v2</span><span class="sxs-lookup"><span data-stu-id="59722-116">MSL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/mapping/cs |
| <span data-ttu-id="59722-117">MSL v3</span><span class="sxs-lookup"><span data-stu-id="59722-117">MSL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a><span data-ttu-id="59722-118">Alias-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="59722-118">Alias Element (MSL)</span></span>

<span data-ttu-id="59722-119">Das **Alias** -Element in der Mapping-Spezifikationssprache (MSL) ist ein untergeordnetes Element des Mapping-Elements, das zum Definieren von Aliasen für das konzeptionelle Modell und die Namespaces des Speicher Modells verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-119">The **Alias** element in mapping specification language (MSL) is a child of the Mapping element that is used to define aliases for conceptual model and storage model namespaces.</span></span> <span data-ttu-id="59722-120">Namen aller konzeptionellen oder Speichermodelltypen, auf denen in MSL verwiesen wird, müssen mit dem jeweiligen Namespacenamen qualifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="59722-120">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="59722-121">Weitere Informationen zum Namespace Namen des konzeptionellen Modells finden Sie unter Schema-Element (CSDL).</span><span class="sxs-lookup"><span data-stu-id="59722-121">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="59722-122">Weitere Informationen zum Namespace Namen des Speicher Modells finden Sie unter Schema-Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="59722-122">For information about the storage model namespace name, see Schema Element (SSDL).</span></span>

<span data-ttu-id="59722-123">Das **Alias** -Element darf keine untergeordneten Elemente aufweisen.</span><span class="sxs-lookup"><span data-stu-id="59722-123">The **Alias** element cannot have child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="59722-124">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="59722-124">Applicable Attributes</span></span>

<span data-ttu-id="59722-125">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **Alias** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="59722-125">The table below describes the attributes that can be applied to the **Alias** element.</span></span>

| <span data-ttu-id="59722-126">Attributname</span><span class="sxs-lookup"><span data-stu-id="59722-126">Attribute Name</span></span> | <span data-ttu-id="59722-127">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="59722-127">Is Required</span></span> | <span data-ttu-id="59722-128">value</span><span class="sxs-lookup"><span data-stu-id="59722-128">Value</span></span>                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| <span data-ttu-id="59722-129">**Schlüssel**</span><span class="sxs-lookup"><span data-stu-id="59722-129">**Key**</span></span>        | <span data-ttu-id="59722-130">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-130">Yes</span></span>         | <span data-ttu-id="59722-131">Der Alias für den Namespace, der durch das **value** -Attribut angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="59722-131">The alias for the namespace that is specified by the **Value** attribute.</span></span> |
| <span data-ttu-id="59722-132">**Wert**</span><span class="sxs-lookup"><span data-stu-id="59722-132">**Value**</span></span>      | <span data-ttu-id="59722-133">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-133">Yes</span></span>         | <span data-ttu-id="59722-134">Der Namespace, für den der Wert des **Schlüssel** Elements ein Alias ist.</span><span class="sxs-lookup"><span data-stu-id="59722-134">The namespace for which the value of the **Key** element is an alias.</span></span>     |

### <a name="example"></a><span data-ttu-id="59722-135">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-135">Example</span></span>

<span data-ttu-id="59722-136">Das folgende Beispiel zeigt ein **Alias** -Element, das einen Alias definiert, `c`für Typen, die im konzeptionellen Modell definiert sind.</span><span class="sxs-lookup"><span data-stu-id="59722-136">The following example shows an **Alias** element that defines an alias, `c`, for types that are defined in the conceptual model.</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="https://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="associationend-element-msl"></a><span data-ttu-id="59722-137">AssociationEnd-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="59722-137">AssociationEnd Element (MSL)</span></span>

<span data-ttu-id="59722-138">Das **AssociationEnd** -Element in der Mapping-Spezifikationssprache (MSL) wird verwendet, wenn die Änderungs Funktionen eines Entitäts Typs im konzeptionellen Modell gespeicherten Prozeduren in der zugrunde liegenden Datenbank zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="59722-138">The **AssociationEnd** element in mapping specification language (MSL) is used when the modification functions of an entity type in the conceptual model are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="59722-139">Wenn eine gespeicherte Änderungs Prozedur einen Parameter annimmt, dessen Wert in einer Association-Eigenschaft enthalten ist, ordnet das **AssociationEnd** -Element den-Eigenschafts Wert dem-Parameter zu.</span><span class="sxs-lookup"><span data-stu-id="59722-139">If a modification stored procedure takes a parameter whose value is held in an association property, the **AssociationEnd** element maps the property value to the parameter.</span></span> <span data-ttu-id="59722-140">Weitere Informationen finden Sie im untenstehenden Beispiel.</span><span class="sxs-lookup"><span data-stu-id="59722-140">For more information, see the example below.</span></span>

<span data-ttu-id="59722-141">Weitere Informationen zum Mapping von Änderungs Funktionen von Entitäts Typen zu gespeicherten Prozeduren finden Sie unter ModificationFunctionMapping-Element (MSL) und Exemplarische Vorgehensweise: Mapping einer Entität zu gespeicherten Prozeduren.</span><span class="sxs-lookup"><span data-stu-id="59722-141">For more information about mapping modification functions of entity types to stored procedures, see ModificationFunctionMapping Element (MSL) and Walkthrough: Mapping an Entity to Stored Procedures.</span></span>

<span data-ttu-id="59722-142">Das **AssociationEnd** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="59722-142">The **AssociationEnd** element can have the following child elements:</span></span>

-   <span data-ttu-id="59722-143">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="59722-143">ScalarProperty</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="59722-144">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="59722-144">Applicable Attributes</span></span>

<span data-ttu-id="59722-145">In der folgenden Tabelle werden die Attribute beschrieben, die für das **AssociationEnd** -Element anwendbar sind.</span><span class="sxs-lookup"><span data-stu-id="59722-145">The following table describes the attributes that are applicable to the **AssociationEnd** element.</span></span>

| <span data-ttu-id="59722-146">Attributname</span><span class="sxs-lookup"><span data-stu-id="59722-146">Attribute Name</span></span>     | <span data-ttu-id="59722-147">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="59722-147">Is Required</span></span> | <span data-ttu-id="59722-148">value</span><span class="sxs-lookup"><span data-stu-id="59722-148">Value</span></span>                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="59722-149">**AssociationSet**</span><span class="sxs-lookup"><span data-stu-id="59722-149">**AssociationSet**</span></span> | <span data-ttu-id="59722-150">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-150">Yes</span></span>         | <span data-ttu-id="59722-151">Der Name der Zuordnung, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-151">The name of the association that is being mapped.</span></span>                                                                                                                                 |
| <span data-ttu-id="59722-152">**From**</span><span class="sxs-lookup"><span data-stu-id="59722-152">**From**</span></span>           | <span data-ttu-id="59722-153">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-153">Yes</span></span>         | <span data-ttu-id="59722-154">Der Wert des **FromRole** -Attributs der Navigations Eigenschaft, die der zugeordneten Zuordnung entspricht.</span><span class="sxs-lookup"><span data-stu-id="59722-154">The value of the **FromRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="59722-155">Weitere Informationen finden Sie unter NavigationProperty-Element (CSDL).</span><span class="sxs-lookup"><span data-stu-id="59722-155">For more information, see NavigationProperty Element (CSDL).</span></span> |
| <span data-ttu-id="59722-156">**An**</span><span class="sxs-lookup"><span data-stu-id="59722-156">**To**</span></span>             | <span data-ttu-id="59722-157">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-157">Yes</span></span>         | <span data-ttu-id="59722-158">Der Wert des Attributs " **Tor** " der Navigations Eigenschaft, die der Zuordnung entspricht, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-158">The value of the **ToRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="59722-159">Weitere Informationen finden Sie unter NavigationProperty-Element (CSDL).</span><span class="sxs-lookup"><span data-stu-id="59722-159">For more information, see NavigationProperty Element (CSDL).</span></span>   |

### <a name="example"></a><span data-ttu-id="59722-160">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-160">Example</span></span>

<span data-ttu-id="59722-161">Betrachten Sie den folgenden Entitätstyp des konzeptionellen Modells:</span><span class="sxs-lookup"><span data-stu-id="59722-161">Consider the following conceptual model entity type:</span></span>

``` xml
 <EntityType Name="Course">
   <Key>
     <PropertyRef Name="CourseID" />
   </Key>
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" MaxLength="100"
             FixedLength="false" Unicode="true" />
   <Property Type="Int32" Name="Credits" Nullable="false" />
   <NavigationProperty Name="Department"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Course" ToRole="Department" />
 </EntityType>
```

<span data-ttu-id="59722-162">Betrachten Sie auch die folgende gespeicherte Prozedur:</span><span class="sxs-lookup"><span data-stu-id="59722-162">Also consider the following stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[UpdateCourse]
                                @CourseID int,
                                @Title nvarchar(50),
                                @Credits int,
                                @DepartmentID int
                                AS
                                UPDATE Course SET Title=@Title,
                                                              Credits=@Credits,
                                                              DepartmentID=@DepartmentID
                                WHERE CourseID=@CourseID;
```

<span data-ttu-id="59722-163">Um die Aktualisierungs Funktion der `Course` Entität dieser gespeicherten Prozedur zuzuordnen, müssen Sie einen Wert für den **DepartmentID** -Parameter angeben.</span><span class="sxs-lookup"><span data-stu-id="59722-163">In order to map the update function of the `Course` entity to this stored procedure, you must supply a value to the **DepartmentID** parameter.</span></span> <span data-ttu-id="59722-164">Der Wert für `DepartmentID` entspricht keiner Eigenschaft des Entitätstyps; er ist vielmehr in einer unabhängigen Zuordnung enthalten, deren Zuordnung hier angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="59722-164">The value for `DepartmentID` does not correspond to a property on the entity type; it is contained in an independent association whose mapping is shown here:</span></span>

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
 </AssociationSetMapping>
```

<span data-ttu-id="59722-165">Der folgende Code zeigt das **AssociationEnd** -Element, das verwendet wird, um die **DepartmentID** -Eigenschaft der **FK-\_Kurs\_Abteilungs** Zuordnung der gespeicherten Prozedur **updatecourse** zuzuordnen (in der die Update-Funktion des **Course** -Entitäts Typs zugeordnet ist):</span><span class="sxs-lookup"><span data-stu-id="59722-165">The following code shows the **AssociationEnd** element used to map the **DepartmentID** property of the **FK\_Course\_Department** association to the **UpdateCourse** stored procedure (to which the update function of the **Course** entity type is mapped):</span></span>

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdateCourse">
         <AssociationEnd AssociationSet="FK_Course_Department"
                         From="Course" To="Department">
           <ScalarProperty Name="DepartmentID"
                           ParameterName="DepartmentID"
                           Version="Current" />
         </AssociationEnd>
         <ScalarProperty Name="Credits" ParameterName="Credits"
                         Version="Current" />
         <ScalarProperty Name="Title" ParameterName="Title"
                         Version="Current" />
         <ScalarProperty Name="CourseID" ParameterName="CourseID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="associationsetmapping-element-msl"></a><span data-ttu-id="59722-166">AssociationSetMapping-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="59722-166">AssociationSetMapping Element (MSL)</span></span>

<span data-ttu-id="59722-167">Das **AssociationSetMapping** -Element in der Mapping-Spezifikationssprache (Mapping Specification Language, MSL) definiert die Zuordnung zwischen einer Zuordnung im konzeptionellen Modell und den Tabellen Spalten in der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="59722-167">The **AssociationSetMapping** element in mapping specification language (MSL) defines the mapping between an association in the conceptual model and table columns in the underlying database.</span></span>

<span data-ttu-id="59722-168">Zuordnungen im konzeptionellen Modell sind Typen, deren Eigenschaften Primär- und Fremdschlüsselspalten in der zugrunde liegenden Datenbank darstellen.</span><span class="sxs-lookup"><span data-stu-id="59722-168">Associations in the conceptual model are types whose properties represent primary and foreign key columns in the underlying database.</span></span> <span data-ttu-id="59722-169">Das **AssociationSetMapping** -Element verwendet zwei EndProperty-Elemente, um die Zuordnungen zwischen Zuordnungstyp Eigenschaften und Spalten in der Datenbank zu definieren.</span><span class="sxs-lookup"><span data-stu-id="59722-169">The **AssociationSetMapping** element uses two EndProperty elements to define the mappings between association type properties and columns in the database.</span></span> <span data-ttu-id="59722-170">Sie können mit dem Condition-Element Bedingungen für diese Zuordnungen festlegen.</span><span class="sxs-lookup"><span data-stu-id="59722-170">You can place conditions on these mappings with the Condition element.</span></span> <span data-ttu-id="59722-171">Mit dem ModificationFunctionMapping-Element ordnen Sie Einfüge-, Update- und Löschfunktionen für Zuordnungen gespeicherten Prozeduren in der Datenbank zu.</span><span class="sxs-lookup"><span data-stu-id="59722-171">Map the insert, update, and delete functions for associations to stored procedures in the database with the ModificationFunctionMapping element.</span></span> <span data-ttu-id="59722-172">Definieren Sie schreibgeschützte Zuordnungen zwischen Zuordnungen und Tabellen Spalten, indem Sie eine Entity SQL Zeichenfolge in einem QueryView-Element verwenden.</span><span class="sxs-lookup"><span data-stu-id="59722-172">Define read-only mappings between associations and table columns by using an Entity SQL string in a QueryView element.</span></span>

> [!NOTE]
> <span data-ttu-id="59722-173">Wenn eine referenzielle Einschränkung für eine Zuordnung im konzeptionellen Modell definiert ist, muss die Zuordnung nicht mit einem **AssociationSetMapping** -Element zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="59722-173">If a referential constraint is defined for an association in the conceptual model, the association does not need to be mapped with an **AssociationSetMapping** element.</span></span> <span data-ttu-id="59722-174">Wenn ein **AssociationSetMapping** -Element für eine Zuordnung vorhanden ist, die eine referenzielle Einschränkung aufweist, werden die im **AssociationSetMapping** -Element definierten Zuordnungen ignoriert.</span><span class="sxs-lookup"><span data-stu-id="59722-174">If an **AssociationSetMapping** element is present for an association that has a referential constraint, the mappings defined in the **AssociationSetMapping** element will be ignored.</span></span> <span data-ttu-id="59722-175">Weitere Informationen finden Sie unter referentialeinschränkungs-Element (CSDL).</span><span class="sxs-lookup"><span data-stu-id="59722-175">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="59722-176">Das **AssociationSetMapping** -Element kann die folgenden untergeordneten Elemente aufweisen.</span><span class="sxs-lookup"><span data-stu-id="59722-176">The **AssociationSetMapping** element can have the following child elements</span></span>

-   <span data-ttu-id="59722-177">QueryView (null oder eins)</span><span class="sxs-lookup"><span data-stu-id="59722-177">QueryView (zero or one)</span></span>
-   <span data-ttu-id="59722-178">EndProperty (kein (null) oder zwei Elemente)</span><span class="sxs-lookup"><span data-stu-id="59722-178">EndProperty (zero or two)</span></span>
-   <span data-ttu-id="59722-179">Bedingung (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-179">Condition (zero or more)</span></span>
-   <span data-ttu-id="59722-180">ModificationFunctionMapping (0 (null) oder 1)</span><span class="sxs-lookup"><span data-stu-id="59722-180">ModificationFunctionMapping (zero or one)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="59722-181">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="59722-181">Applicable Attributes</span></span>

<span data-ttu-id="59722-182">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **AssociationSetMapping** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="59722-182">The following table describes the attributes that can be applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="59722-183">Attributname</span><span class="sxs-lookup"><span data-stu-id="59722-183">Attribute Name</span></span>     | <span data-ttu-id="59722-184">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="59722-184">Is Required</span></span> | <span data-ttu-id="59722-185">value</span><span class="sxs-lookup"><span data-stu-id="59722-185">Value</span></span>                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| <span data-ttu-id="59722-186">**Name**</span><span class="sxs-lookup"><span data-stu-id="59722-186">**Name**</span></span>           | <span data-ttu-id="59722-187">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-187">Yes</span></span>         | <span data-ttu-id="59722-188">Der Name des konzeptionellen Modell-Zuordnungssatzes, der zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-188">The name of the conceptual model association set that is being mapped.</span></span>                      |
| <span data-ttu-id="59722-189">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="59722-189">**TypeName**</span></span>       | <span data-ttu-id="59722-190">Nein</span><span class="sxs-lookup"><span data-stu-id="59722-190">No</span></span>          | <span data-ttu-id="59722-191">Der mit einem Namespace qualifizierte Name des konzeptionellen Modell-Zuordnungstyps, der zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-191">The namespace-qualified name of the conceptual model association type that is being mapped.</span></span> |
| <span data-ttu-id="59722-192">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="59722-192">**StoreEntitySet**</span></span> | <span data-ttu-id="59722-193">Nein</span><span class="sxs-lookup"><span data-stu-id="59722-193">No</span></span>          | <span data-ttu-id="59722-194">Der Name der Tabelle, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-194">The name of the table that is being mapped.</span></span>                                                 |

### <a name="example"></a><span data-ttu-id="59722-195">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-195">Example</span></span>

<span data-ttu-id="59722-196">Das folgende Beispiel zeigt ein **AssociationSetMapping** -Element, in dem der **FK-\_Kurs\_Abteilungs** Zuordnungs Satz im konzeptionellen Modell der **Course** -Tabelle in der-Datenbank zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="59722-196">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association set in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="59722-197">Zuordnungen zwischen Zuordnungstyp Eigenschaften und Tabellen Spalten werden in untergeordneten **EndProperty** -Elementen angegeben.</span><span class="sxs-lookup"><span data-stu-id="59722-197">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
```

## <a name="complexproperty-element-msl"></a><span data-ttu-id="59722-198">ComplexProperty-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="59722-198">ComplexProperty Element (MSL)</span></span>

<span data-ttu-id="59722-199">Ein **ComplexProperty** -Element in der Mapping-Spezifikationssprache (MSL) definiert die Zuordnung zwischen einer komplexen Typeigenschaft in einem Entitätstyp des konzeptionellen Modells und Tabellen Spalten in der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="59722-199">A **ComplexProperty** element in mapping specification language (MSL) defines the mapping between a complex type property on a conceptual model entity type and table columns in the underlying database.</span></span> <span data-ttu-id="59722-200">Die Zuordnungen zu Eigenschaftenspalten sind in einem untergeordneten ScalarProperty-Element angegeben.</span><span class="sxs-lookup"><span data-stu-id="59722-200">The property-column mappings are specified in child ScalarProperty elements.</span></span>

<span data-ttu-id="59722-201">Das **complexType** -Eigenschafts Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="59722-201">The **ComplexType** property element can have the following child elements:</span></span>

-   <span data-ttu-id="59722-202">ScalarProperty (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-202">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="59722-203">**ComplexProperty** (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-203">**ComplexProperty** (zero or more)</span></span>
-   <span data-ttu-id="59722-204">Complexttypeer Mapping (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-204">ComplextTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="59722-205">Bedingung (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-205">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="59722-206">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="59722-206">Applicable Attributes</span></span>

<span data-ttu-id="59722-207">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **ComplexProperty** -Element anwendbar sind:</span><span class="sxs-lookup"><span data-stu-id="59722-207">The following table describes the attributes that are applicable to the **ComplexProperty** element:</span></span>

| <span data-ttu-id="59722-208">Attributname</span><span class="sxs-lookup"><span data-stu-id="59722-208">Attribute Name</span></span> | <span data-ttu-id="59722-209">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="59722-209">Is Required</span></span> | <span data-ttu-id="59722-210">value</span><span class="sxs-lookup"><span data-stu-id="59722-210">Value</span></span>                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="59722-211">**Name**</span><span class="sxs-lookup"><span data-stu-id="59722-211">**Name**</span></span>       | <span data-ttu-id="59722-212">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-212">Yes</span></span>         | <span data-ttu-id="59722-213">Der Name der komplexen Eigenschaft eines Entitätstyps im konzeptionellen Modell, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-213">The name of the complex property of an entity type in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="59722-214">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="59722-214">**TypeName**</span></span>   | <span data-ttu-id="59722-215">Nein</span><span class="sxs-lookup"><span data-stu-id="59722-215">No</span></span>          | <span data-ttu-id="59722-216">Der mit einem Namespace qualifizierte Name des Eigenschaftentyps im konzeptionellen Modell.</span><span class="sxs-lookup"><span data-stu-id="59722-216">The namespace-qualified name of the conceptual model property type.</span></span>                              |

### <a name="example"></a><span data-ttu-id="59722-217">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-217">Example</span></span>

<span data-ttu-id="59722-218">Das folgende Beispiel beruht auf dem School-Modell.</span><span class="sxs-lookup"><span data-stu-id="59722-218">The following example is based on the School model.</span></span> <span data-ttu-id="59722-219">Dem konzeptionellen Modell wurde der folgende komplexe Typ hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="59722-219">The following complex type has been added to the conceptual model:</span></span>

``` xml
 <ComplexType Name="FullName">
   <Property Type="String" Name="LastName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
   <Property Type="String" Name="FirstName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
 </ComplexType>
```

<span data-ttu-id="59722-220">Die Eigenschaften " **LastName** " und " **FirstName** " des Entitäts Typs " **Person** " wurden durch eine komplexe Eigenschaft **namens "Name**" ersetzt:</span><span class="sxs-lookup"><span data-stu-id="59722-220">The **LastName** and **FirstName** properties of the **Person** entity type have been replaced with one complex property, **Name**:</span></span>

``` xml
 <EntityType Name="Person">
   <Key>
     <PropertyRef Name="PersonID" />
   </Key>
   <Property Name="PersonID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="HireDate" Type="DateTime" />
   <Property Name="EnrollmentDate" Type="DateTime" />
   <Property Name="Name" Type="SchoolModel.FullName" Nullable="false" />
 </EntityType>
```

<span data-ttu-id="59722-221">Das folgende MSL zeigt das **ComplexProperty** -Element, das verwendet wird, um die **Name** -Eigenschaft Spalten in der zugrunde liegenden Datenbank zuzuordnen:</span><span class="sxs-lookup"><span data-stu-id="59722-221">The following MSL shows the **ComplexProperty** element used to map the **Name** property to columns in the underlying database:</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
       <ComplexProperty Name="Name" TypeName="SchoolModel.FullName">
         <ScalarProperty Name="FirstName" ColumnName="FirstName" />
         <ScalarProperty Name="LastName" ColumnName="LastName" />  
       </ComplexProperty>
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="complextypemapping-element-msl"></a><span data-ttu-id="59722-222">ComplexTypeMapping-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="59722-222">ComplexTypeMapping Element (MSL)</span></span>

<span data-ttu-id="59722-223">Das **complextypemapping** -Element in der Mapping-Spezifikationssprache (MSL) ist ein untergeordnetes Element des resultmapping-Elements und definiert die Zuordnung zwischen einem Funktions Import im konzeptionellen Modell und einer gespeicherten Prozedur in der zugrunde liegenden Datenbank, wenn Folgendes zutrifft:</span><span class="sxs-lookup"><span data-stu-id="59722-223">The **ComplexTypeMapping** element in mapping specification language (MSL) is a child of the ResultMapping element and defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="59722-224">Der Funktionsimport gibt einen konzeptionellen komplexen Typ zurück.</span><span class="sxs-lookup"><span data-stu-id="59722-224">The function import returns a conceptual complex type.</span></span>
-   <span data-ttu-id="59722-225">Die Namen der Spalten, die von der gespeicherten Prozedur zurückgegeben werden, entsprechen nicht genau den Namen der Eigenschaften für den komplexen Typ.</span><span class="sxs-lookup"><span data-stu-id="59722-225">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the complex type.</span></span>

<span data-ttu-id="59722-226">Standardmäßig basiert die Zuordnung der von einer gespeicherten Prozedur zurückgegebenen Spalten zu einem komplexen Typ auf den Spalten- und Eigenschaftennamen.</span><span class="sxs-lookup"><span data-stu-id="59722-226">By default, the mapping between the columns returned by a stored procedure and a complex type is based on column and property names.</span></span> <span data-ttu-id="59722-227">Wenn Spaltennamen nicht exakt mit Eigenschaftsnamen übereinstimmen, müssen Sie das **complextypemapping** -Element verwenden, um die Zuordnung zu definieren.</span><span class="sxs-lookup"><span data-stu-id="59722-227">If column names do not exactly match property names, you must use the **ComplexTypeMapping** element to define the mapping.</span></span> <span data-ttu-id="59722-228">Ein Beispiel für die Standard Zuordnung finden Sie unter FunctionImportMapping-Element (MSL).</span><span class="sxs-lookup"><span data-stu-id="59722-228">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="59722-229">Das **complextypemapping** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="59722-229">The **ComplexTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="59722-230">ScalarProperty (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-230">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="59722-231">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="59722-231">Applicable Attributes</span></span>

<span data-ttu-id="59722-232">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **complextypemapping** -Element anwendbar sind.</span><span class="sxs-lookup"><span data-stu-id="59722-232">The following table describes the attributes that are applicable to the **ComplexTypeMapping** element.</span></span>

| <span data-ttu-id="59722-233">Attributname</span><span class="sxs-lookup"><span data-stu-id="59722-233">Attribute Name</span></span> | <span data-ttu-id="59722-234">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="59722-234">Is Required</span></span> | <span data-ttu-id="59722-235">value</span><span class="sxs-lookup"><span data-stu-id="59722-235">Value</span></span>                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| <span data-ttu-id="59722-236">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="59722-236">**TypeName**</span></span>   | <span data-ttu-id="59722-237">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-237">Yes</span></span>         | <span data-ttu-id="59722-238">Der namespacequalifizierte Name des komplexen Typs, der zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-238">The namespace-qualified name of the complex type that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="59722-239">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-239">Example</span></span>

<span data-ttu-id="59722-240">Sehen Sie sich die folgende gespeicherte Prozedur an:</span><span class="sxs-lookup"><span data-stu-id="59722-240">Consider the following stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

<span data-ttu-id="59722-241">Betrachten Sie auch den folgenden komplexen Typ des konzeptionellen Modells:</span><span class="sxs-lookup"><span data-stu-id="59722-241">Also consider the following conceptual model complex type:</span></span>

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

<span data-ttu-id="59722-242">Um einen Funktions Import zu erstellen, der Instanzen des vorherigen komplexen Typs zurückgibt, muss die Zuordnung zwischen den Spalten, die von der gespeicherten Prozedur zurückgegeben werden, und dem Entitätstyp in einem **complextypemapping** -Element definiert werden:</span><span class="sxs-lookup"><span data-stu-id="59722-242">In order to create a function import that returns instances of the previous complex type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ComplexTypeMapping** element:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <ComplexTypeMapping TypeName="SchoolModel.GradeInfo">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </ComplexTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="condition-element-msl"></a><span data-ttu-id="59722-243">Condition-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="59722-243">Condition Element (MSL)</span></span>

<span data-ttu-id="59722-244">Das **Condition** -Element in der Mapping-Spezifikationssprache (Mapping Specification Language, MSL) ordnet Bedingungen für Zuordnungen zwischen dem konzeptionellen Modell und der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="59722-244">The **Condition** element in mapping specification language (MSL) places conditions on mappings between the conceptual model and the underlying database.</span></span> <span data-ttu-id="59722-245">Die Zuordnung, die in einem XML-Knoten definiert ist, ist gültig, wenn alle Bedingungen, die in den untergeordneten **Bedingungs Elementen angegeben** sind, erfüllt werden.</span><span class="sxs-lookup"><span data-stu-id="59722-245">The mapping that is defined within an XML node is valid if all conditions, as specified in child **Condition** elements, are met.</span></span> <span data-ttu-id="59722-246">Andernfalls ist die Zuordnung ungültig.</span><span class="sxs-lookup"><span data-stu-id="59722-246">Otherwise, the mapping is not valid.</span></span> <span data-ttu-id="59722-247">Wenn ein MappingFragment-Element z. b. ein oder **mehrere unter** geordnete Bedingungs Elemente enthält, ist die im Knoten **MappingFragment** definierte Zuordnung nur gültig, wenn alle Bedingungen der untergeordneten **Bedingungs Elemente erfüllt** sind.</span><span class="sxs-lookup"><span data-stu-id="59722-247">For example, if a MappingFragment element contains one or more **Condition** child elements, the mapping defined within the **MappingFragment** node will only be valid if all the conditions of the child **Condition** elements are met.</span></span>

<span data-ttu-id="59722-248">Jede Bedingung kann entweder auf einen **Namen** (den Namen einer Entitäts Eigenschaft des konzeptionellen Modells, der durch das **Name** -Attribut festgelegt ist) oder auf einen **ColumnName** (der Name einer Spalte in der Datenbank, die durch das **ColumnName** -Attribut angegeben ist) angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="59722-248">Each condition can apply to either a **Name** (the name of a conceptual model entity property, specified by the **Name** attribute), or a **ColumnName** (the name of a column in the database, specified by the **ColumnName** attribute).</span></span> <span data-ttu-id="59722-249">Wenn das **Name** -Attribut festgelegt ist, wird die Bedingung anhand eines Entitäts Eigenschafts Werts überprüft.</span><span class="sxs-lookup"><span data-stu-id="59722-249">When the **Name** attribute is set, the condition is checked against an entity property value.</span></span> <span data-ttu-id="59722-250">Wenn das **ColumnName** -Attribut festgelegt ist, wird die Bedingung anhand eines Spaltenwerts überprüft.</span><span class="sxs-lookup"><span data-stu-id="59722-250">When the **ColumnName** attribute is set, the condition is checked against a column value.</span></span> <span data-ttu-id="59722-251">Nur eines der Attribute " **Name** " oder " **ColumnName** " kann in einem **Condition** -Element angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="59722-251">Only one of the **Name** or **ColumnName** attribute can be specified in a **Condition** element.</span></span>

> [!NOTE]
> <span data-ttu-id="59722-252">Wenn das **Condition** -Element innerhalb eines FunctionImportMapping-Elements verwendet wird, ist nur das **Name** -Attribut anwendbar.</span><span class="sxs-lookup"><span data-stu-id="59722-252">When the **Condition** element is used within a FunctionImportMapping element, only the **Name** attribute is not applicable.</span></span>

<span data-ttu-id="59722-253">Das **Condition** -Element kann ein untergeordnetes Element der folgenden Elemente sein:</span><span class="sxs-lookup"><span data-stu-id="59722-253">The **Condition** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="59722-254">AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="59722-254">AssociationSetMapping</span></span>
-   <span data-ttu-id="59722-255">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="59722-255">ComplexProperty</span></span>
-   <span data-ttu-id="59722-256">EntitySetMapping</span><span class="sxs-lookup"><span data-stu-id="59722-256">EntitySetMapping</span></span>
-   <span data-ttu-id="59722-257">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="59722-257">MappingFragment</span></span>
-   <span data-ttu-id="59722-258">EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="59722-258">EntityTypeMapping</span></span>

<span data-ttu-id="59722-259">Das **Condition** -Element kann keine untergeordneten Elemente aufweisen.</span><span class="sxs-lookup"><span data-stu-id="59722-259">The **Condition** element can have no child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="59722-260">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="59722-260">Applicable Attributes</span></span>

<span data-ttu-id="59722-261">In der folgenden Tabelle werden die Attribute beschrieben, die für das **Condition** -Element anwendbar sind:</span><span class="sxs-lookup"><span data-stu-id="59722-261">The following table describes the attributes that are applicable to the **Condition** element:</span></span>

| <span data-ttu-id="59722-262">Attributname</span><span class="sxs-lookup"><span data-stu-id="59722-262">Attribute Name</span></span> | <span data-ttu-id="59722-263">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="59722-263">Is Required</span></span> | <span data-ttu-id="59722-264">value</span><span class="sxs-lookup"><span data-stu-id="59722-264">Value</span></span>                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="59722-265">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="59722-265">**ColumnName**</span></span> | <span data-ttu-id="59722-266">Nein</span><span class="sxs-lookup"><span data-stu-id="59722-266">No</span></span>          | <span data-ttu-id="59722-267">Der Name der Tabellenspalte, deren Wert zur Auswertung der Bedingung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-267">The name of the table column whose value is used to evaluate the condition.</span></span>                                                                                                                                                                                                                   |
| <span data-ttu-id="59722-268">**IsNull**</span><span class="sxs-lookup"><span data-stu-id="59722-268">**IsNull**</span></span>     | <span data-ttu-id="59722-269">Nein</span><span class="sxs-lookup"><span data-stu-id="59722-269">No</span></span>          | <span data-ttu-id="59722-270">**True** und **False**.</span><span class="sxs-lookup"><span data-stu-id="59722-270">**True** or **False**.</span></span> <span data-ttu-id="59722-271">Wenn der Wert **true** und der Spaltenwert **null**ist, oder wenn der Wert **false** ist und der Spaltenwert nicht **null**ist, ist die Bedingung true.</span><span class="sxs-lookup"><span data-stu-id="59722-271">If the value is **True** and the column value is **null**, or if the value is **False** and the column value is not **null**, the condition is true.</span></span> <span data-ttu-id="59722-272">Andernfalls ist die Bedingung nicht erfüllt (false).</span><span class="sxs-lookup"><span data-stu-id="59722-272">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="59722-273">Die Attribute " **IsNull** " und " **value** " können nicht gleichzeitig verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="59722-273">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span> |
| <span data-ttu-id="59722-274">**Wert**</span><span class="sxs-lookup"><span data-stu-id="59722-274">**Value**</span></span>      | <span data-ttu-id="59722-275">Nein</span><span class="sxs-lookup"><span data-stu-id="59722-275">No</span></span>          | <span data-ttu-id="59722-276">Der Wert, mit dem der Spaltenwert verglichen werden soll.</span><span class="sxs-lookup"><span data-stu-id="59722-276">The value with which the column value is compared.</span></span> <span data-ttu-id="59722-277">Wenn die Werte gleich sind, wird die Bedingung erfüllt (true).</span><span class="sxs-lookup"><span data-stu-id="59722-277">If the values are the same, the condition is true.</span></span> <span data-ttu-id="59722-278">Andernfalls ist die Bedingung nicht erfüllt (false).</span><span class="sxs-lookup"><span data-stu-id="59722-278">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="59722-279">Die Attribute " **IsNull** " und " **value** " können nicht gleichzeitig verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="59722-279">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span>                                                                       |
| <span data-ttu-id="59722-280">**Name**</span><span class="sxs-lookup"><span data-stu-id="59722-280">**Name**</span></span>       | <span data-ttu-id="59722-281">Nein</span><span class="sxs-lookup"><span data-stu-id="59722-281">No</span></span>          | <span data-ttu-id="59722-282">Der Name der Entitätseigenschaft im konzeptionellen Modell, deren Wert zur Auswertung der Bedingung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-282">The name of the conceptual model entity property whose value is used to evaluate the condition.</span></span> <br/> <span data-ttu-id="59722-283">Dieses Attribut ist nicht anwendbar, wenn das **Condition** -Element innerhalb eines FunctionImportMapping-Elements verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-283">This attribute is not applicable if the **Condition** element is used within a FunctionImportMapping element.</span></span>                                                                           |

### <a name="example"></a><span data-ttu-id="59722-284">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-284">Example</span></span>

<span data-ttu-id="59722-285">Das folgende **Beispiel zeigt Bedingungs** Elemente als untergeordnete Elemente der **MappingFragment** -Elemente.</span><span class="sxs-lookup"><span data-stu-id="59722-285">The following example shows **Condition** elements as children of **MappingFragment** elements.</span></span> <span data-ttu-id="59722-286">Wenn **HireDate** nicht NULL ist und " **registrimentdate** " den Wert NULL hat, werden Daten zwischen dem Typ " **SchoolModel. Instructor** " und den Spalten **PersonID** und **HireDate** der **Person** -Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="59722-286">When **HireDate** is not null and **EnrollmentDate** is null, data is mapped between the **SchoolModel.Instructor** type and the **PersonID** and **HireDate** columns of the **Person** table.</span></span> <span data-ttu-id="59722-287">Wenn **registrimentdate** nicht NULL und **HireDate** den Wert NULL hat, werden Daten zwischen dem Typ **SchoolModel. Student** **und den Spalten** **PersonID** und Registrierung der **Person** -Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="59722-287">When **EnrollmentDate** is not null and **HireDate** is null, data is mapped between the **SchoolModel.Student** type and the **PersonID** and **Enrollment** columns of the **Person** table.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="deletefunction-element-msl"></a><span data-ttu-id="59722-288">DeleteFunction-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="59722-288">DeleteFunction Element (MSL)</span></span>

<span data-ttu-id="59722-289">Das **DeleteFunction** -Element in der Mapping-Spezifikationssprache (Mapping Specification Language, MSL) ordnet die Delete-Funktion eines Entitäts Typs oder einer Zuordnung im konzeptionellen Modell einer gespeicherten Prozedur in der zugrunde liegenden Datenbank zu.</span><span class="sxs-lookup"><span data-stu-id="59722-289">The **DeleteFunction** element in mapping specification language (MSL) maps the delete function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="59722-290">Gespeicherte Prozeduren, denen Änderungsfunktionen zugeordnet werden, müssen im Speichermodell deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="59722-290">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="59722-291">Weitere Informationen finden Sie unter Function-Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="59722-291">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="59722-292">Wenn Sie nicht alle drei Einfüge-, Aktualisierungs-oder Löschvorgänge eines Entitäts Typs gespeicherten Prozeduren zuordnen, schlagen die nicht zugeordneten Vorgänge fehl, wenn Sie zur Laufzeit ausgeführt werden und eine Update Exception ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="59722-292">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

### <a name="deletefunction-applied-to-entitytypemapping"></a><span data-ttu-id="59722-293">DeleteFunction angewendet auf EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="59722-293">DeleteFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="59722-294">Wenn das **DeleteFunction** -Element auf das EntityTypeMapping-Element angewendet wird, ordnet es die Delete-Funktion eines Entitäts Typs im konzeptionellen Modell einer gespeicherten Prozedur zu.</span><span class="sxs-lookup"><span data-stu-id="59722-294">When applied to the EntityTypeMapping element, the **DeleteFunction** element maps the delete function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="59722-295">Das **DeleteFunction** -Element kann die folgenden untergeordneten Elemente aufweisen, wenn es auf ein **EntityTypeMapping** -Element angewendet wird:</span><span class="sxs-lookup"><span data-stu-id="59722-295">The **DeleteFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="59722-296">AssociationEnd (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-296">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="59722-297">ComplexProperty (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-297">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="59722-298">Scarlarproperty (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-298">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="59722-299">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="59722-299">Applicable Attributes</span></span>

<span data-ttu-id="59722-300">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **DeleteFunction** -Element angewendet werden können, wenn es auf ein **EntityTypeMapping** -Element angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-300">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="59722-301">Attributname</span><span class="sxs-lookup"><span data-stu-id="59722-301">Attribute Name</span></span>            | <span data-ttu-id="59722-302">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="59722-302">Is Required</span></span> | <span data-ttu-id="59722-303">value</span><span class="sxs-lookup"><span data-stu-id="59722-303">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="59722-304">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="59722-304">**FunctionName**</span></span>          | <span data-ttu-id="59722-305">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-305">Yes</span></span>         | <span data-ttu-id="59722-306">Der mit einem Namespace qualifizierte Name der gespeicherten Prozedur, der die Löschfunktion zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-306">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="59722-307">Die gespeicherte Prozedur muss im Speichermodell deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="59722-307">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="59722-308">**Rowsaffectedparameter**</span><span class="sxs-lookup"><span data-stu-id="59722-308">**RowsAffectedParameter**</span></span> | <span data-ttu-id="59722-309">Nein</span><span class="sxs-lookup"><span data-stu-id="59722-309">No</span></span>          | <span data-ttu-id="59722-310">Der Name des Ausgabeparameters, der die Anzahl der betroffenen Zeilen zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="59722-310">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="59722-311">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-311">Example</span></span>

<span data-ttu-id="59722-312">Das folgende Beispiel basiert auf dem Modell "School" und zeigt das **DeleteFunction** -Element, das die Delete-Funktion des Entitäts Typs **Person** der gespeicherten Prozedur **DeletePerson** entspricht.</span><span class="sxs-lookup"><span data-stu-id="59722-312">The following example is based on the School model and shows the **DeleteFunction** element mapping the delete function of the **Person** entity type to the **DeletePerson** stored procedure.</span></span> <span data-ttu-id="59722-313">Die gespeicherte Prozedur **DeletePerson** wird im Speichermodell deklariert.</span><span class="sxs-lookup"><span data-stu-id="59722-313">The **DeletePerson** stored procedure is declared in the storage model.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="deletefunction-applied-to-associationsetmapping"></a><span data-ttu-id="59722-314">DeleteFunction angewendet auf AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="59722-314">DeleteFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="59722-315">Wenn das **DeleteFunction** -Element auf das AssociationSetMapping-Element angewendet wird, ordnet es die Delete-Funktion einer Zuordnung im konzeptionellen Modell einer gespeicherten Prozedur zu.</span><span class="sxs-lookup"><span data-stu-id="59722-315">When applied to the AssociationSetMapping element, the **DeleteFunction** element maps the delete function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="59722-316">Das **DeleteFunction** -Element kann die folgenden untergeordneten Elemente aufweisen, wenn es auf das **AssociationSetMapping** -Element angewendet wird:</span><span class="sxs-lookup"><span data-stu-id="59722-316">The **DeleteFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="59722-317">EndProperty</span><span class="sxs-lookup"><span data-stu-id="59722-317">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="59722-318">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="59722-318">Applicable Attributes</span></span>

<span data-ttu-id="59722-319">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **DeleteFunction** -Element angewendet werden können, wenn es auf das **AssociationSetMapping** -Element angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-319">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="59722-320">Attributname</span><span class="sxs-lookup"><span data-stu-id="59722-320">Attribute Name</span></span>            | <span data-ttu-id="59722-321">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="59722-321">Is Required</span></span> | <span data-ttu-id="59722-322">value</span><span class="sxs-lookup"><span data-stu-id="59722-322">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="59722-323">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="59722-323">**FunctionName**</span></span>          | <span data-ttu-id="59722-324">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-324">Yes</span></span>         | <span data-ttu-id="59722-325">Der mit einem Namespace qualifizierte Name der gespeicherten Prozedur, der die Löschfunktion zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-325">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="59722-326">Die gespeicherte Prozedur muss im Speichermodell deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="59722-326">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="59722-327">**Rowsaffectedparameter**</span><span class="sxs-lookup"><span data-stu-id="59722-327">**RowsAffectedParameter**</span></span> | <span data-ttu-id="59722-328">Nein</span><span class="sxs-lookup"><span data-stu-id="59722-328">No</span></span>          | <span data-ttu-id="59722-329">Der Name des Ausgabeparameters, der die Anzahl der betroffenen Zeilen zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="59722-329">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="59722-330">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-330">Example</span></span>

<span data-ttu-id="59722-331">Das folgende Beispiel basiert auf dem Modell "School" und zeigt das **DeleteFunction** -Element, das verwendet wird, um die Delete-Funktion der " **CourseInstructor** "-Zuordnung der gespeicherten Prozedur " **deletecourseinstructor** " zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="59722-331">The following example is based on the School model and shows the **DeleteFunction** element used to map delete function of the **CourseInstructor** association to the **DeleteCourseInstructor** stored procedure.</span></span> <span data-ttu-id="59722-332">Die gespeicherte Prozedur **deletecourseinstructor** ist im Speichermodell deklariert.</span><span class="sxs-lookup"><span data-stu-id="59722-332">The **DeleteCourseInstructor** stored procedure is declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="endproperty-element-msl"></a><span data-ttu-id="59722-333">EndProperty-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="59722-333">EndProperty Element (MSL)</span></span>

<span data-ttu-id="59722-334">Das **EndProperty** -Element in der Mapping-Spezifikationssprache (Mapping Specification Language, MSL) definiert die Zuordnung zwischen einer End-oder einer Änderungs Funktion einer konzeptionellen Modell Zuordnung und der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="59722-334">The **EndProperty** element in mapping specification language (MSL) defines the mapping between an end or a modification function of a conceptual model association and the underlying database.</span></span> <span data-ttu-id="59722-335">Die Zuordnung zur Eigenschaftenspalte ist in einem untergeordneten ScalarProperty-Element angegeben.</span><span class="sxs-lookup"><span data-stu-id="59722-335">The property-column mapping is specified in a child ScalarProperty element.</span></span>

<span data-ttu-id="59722-336">Wenn ein **EndProperty** -Element verwendet wird, um die Zuordnung für das Ende einer konzeptionellen Modell Zuordnung zu definieren, ist es ein untergeordnetes Element eines AssociationSetMapping-Elements.</span><span class="sxs-lookup"><span data-stu-id="59722-336">When an **EndProperty** element is used to define the mapping for the end of a conceptual model association, it is a child of an AssociationSetMapping element.</span></span> <span data-ttu-id="59722-337">Wenn das **EndProperty** -Element verwendet wird, um die Zuordnung für eine Änderungs Funktion einer konzeptionellen Modell Zuordnung zu definieren, ist es ein untergeordnetes Element eines InsertFunction-Elements oder eines DeleteFunction-Elements.</span><span class="sxs-lookup"><span data-stu-id="59722-337">When the **EndProperty** element is used to define the mapping for a modification function of a conceptual model association, it is a child of an InsertFunction element or DeleteFunction element.</span></span>

<span data-ttu-id="59722-338">Das **EndProperty** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="59722-338">The **EndProperty** element can have the following child elements:</span></span>

-   <span data-ttu-id="59722-339">ScalarProperty (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-339">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="59722-340">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="59722-340">Applicable Attributes</span></span>

<span data-ttu-id="59722-341">In der folgenden Tabelle werden die Attribute beschrieben, die für das **EndProperty** -Element anwendbar sind:</span><span class="sxs-lookup"><span data-stu-id="59722-341">The following table describes the attributes that are applicable to the **EndProperty** element:</span></span>

| <span data-ttu-id="59722-342">Attributname</span><span class="sxs-lookup"><span data-stu-id="59722-342">Attribute Name</span></span> | <span data-ttu-id="59722-343">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="59722-343">Is Required</span></span> | <span data-ttu-id="59722-344">value</span><span class="sxs-lookup"><span data-stu-id="59722-344">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="59722-345">Name</span><span class="sxs-lookup"><span data-stu-id="59722-345">Name</span></span>           | <span data-ttu-id="59722-346">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-346">Yes</span></span>         | <span data-ttu-id="59722-347">Der Name des Zuordnungsendes, das zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-347">The name of the association end that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="59722-348">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-348">Example</span></span>

<span data-ttu-id="59722-349">Das folgende Beispiel zeigt ein **AssociationSetMapping** -Element, in dem der **FK-\_Kurs\_Abteilungs** Zuordnung im konzeptionellen Modell der **Course** -Tabelle in der-Datenbank zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="59722-349">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="59722-350">Zuordnungen zwischen Zuordnungstyp Eigenschaften und Tabellen Spalten werden in untergeordneten **EndProperty** -Elementen angegeben.</span><span class="sxs-lookup"><span data-stu-id="59722-350">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
```

### <a name="example"></a><span data-ttu-id="59722-351">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-351">Example</span></span>

<span data-ttu-id="59722-352">Das folgende Beispiel zeigt das **EndProperty** -Element, das die INSERT-und DELETE-Funktionen einer Association (**CourseInstructor**) zu gespeicherten Prozeduren in der zugrunde liegenden Datenbank zuordnet.</span><span class="sxs-lookup"><span data-stu-id="59722-352">The following example shows the **EndProperty** element mapping the insert and delete functions of an association (**CourseInstructor**) to stored procedures in the underlying database.</span></span> <span data-ttu-id="59722-353">Die Funktionen, denen sie zugeordnet werden, sind im Speichermodell deklariert.</span><span class="sxs-lookup"><span data-stu-id="59722-353">The functions that are mapped to are declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="entitycontainermapping-element-msl"></a><span data-ttu-id="59722-354">EntityContainerMapping-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="59722-354">EntityContainerMapping Element (MSL)</span></span>

<span data-ttu-id="59722-355">Das **EntityContainerMapping** -Element in der Mapping-Spezifikationssprache (Mapping Specification Language, MSL) ordnet den Entitäts Container im konzeptionellen Modell dem Entitäts Container im Speichermodell zu.</span><span class="sxs-lookup"><span data-stu-id="59722-355">The **EntityContainerMapping** element in mapping specification language (MSL) maps the entity container in the conceptual model to the entity container in the storage model.</span></span> <span data-ttu-id="59722-356">Das **EntityContainerMapping** -Element ist ein untergeordnetes Element des Mapping-Elements.</span><span class="sxs-lookup"><span data-stu-id="59722-356">The **EntityContainerMapping** element is a child of the Mapping element.</span></span>

<span data-ttu-id="59722-357">Das **EntityContainerMapping** -Element kann die folgenden untergeordneten Elemente aufweisen (in der angegebenen Reihenfolge):</span><span class="sxs-lookup"><span data-stu-id="59722-357">The **EntityContainerMapping** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="59722-358">EntitySetMapping (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-358">EntitySetMapping (zero or more)</span></span>
-   <span data-ttu-id="59722-359">AssociationSetMapping (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-359">AssociationSetMapping (zero or more)</span></span>
-   <span data-ttu-id="59722-360">FunctionImportMapping (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-360">FunctionImportMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="59722-361">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="59722-361">Applicable Attributes</span></span>

<span data-ttu-id="59722-362">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **EntityContainerMapping** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="59722-362">The following table describes the attributes that can be applied to the **EntityContainerMapping** element.</span></span>

| <span data-ttu-id="59722-363">Attributname</span><span class="sxs-lookup"><span data-stu-id="59722-363">Attribute Name</span></span>            | <span data-ttu-id="59722-364">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="59722-364">Is Required</span></span> | <span data-ttu-id="59722-365">value</span><span class="sxs-lookup"><span data-stu-id="59722-365">Value</span></span>                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="59722-366">**Storagemodelcontainer**</span><span class="sxs-lookup"><span data-stu-id="59722-366">**StorageModelContainer**</span></span> | <span data-ttu-id="59722-367">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-367">Yes</span></span>         | <span data-ttu-id="59722-368">Der Name des Entitätscontainers im Speichermodell, der zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-368">The name of the storage model entity container that is being mapped.</span></span>                                                                                                                                                                                     |
| <span data-ttu-id="59722-369">**CdmEntityContainer**</span><span class="sxs-lookup"><span data-stu-id="59722-369">**CdmEntityContainer**</span></span>    | <span data-ttu-id="59722-370">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-370">Yes</span></span>         | <span data-ttu-id="59722-371">Der Name des Entitätscontainers im konzeptionellen Modell, der zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-371">The name of the conceptual model entity container that is being mapped.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="59722-372">**Generateupdateviews**</span><span class="sxs-lookup"><span data-stu-id="59722-372">**GenerateUpdateViews**</span></span>   | <span data-ttu-id="59722-373">Nein</span><span class="sxs-lookup"><span data-stu-id="59722-373">No</span></span>          | <span data-ttu-id="59722-374">**True** und **False**.</span><span class="sxs-lookup"><span data-stu-id="59722-374">**True** or **False**.</span></span> <span data-ttu-id="59722-375">**False**gibt an, dass keine Update Sichten generiert werden.</span><span class="sxs-lookup"><span data-stu-id="59722-375">If **False**, no update views are generated.</span></span> <span data-ttu-id="59722-376">Dieses Attribut sollte auf **false** festgelegt werden, wenn Sie eine schreibgeschützte Zuordnung haben, die ungültig wäre, da die Daten möglicherweise nicht erfolgreich abgerundet werden.</span><span class="sxs-lookup"><span data-stu-id="59722-376">This attribute should be set to **False** when you have a read-only mapping that would be invalid because data may not round-trip successfully.</span></span> <br/> <span data-ttu-id="59722-377">Der Standardwert lautet **True**.</span><span class="sxs-lookup"><span data-stu-id="59722-377">The default value is **True**.</span></span> |

### <a name="example"></a><span data-ttu-id="59722-378">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-378">Example</span></span>

<span data-ttu-id="59722-379">Das folgende Beispiel zeigt ein **EntityContainerMapping** -Element, das den Container **schoolmodelentities** (der Entitäts Container des konzeptionellen Modells) dem Container **schoolmodelstorecontainer** (dem Entitätencontainer des Speicher Modells) zuordnet:</span><span class="sxs-lookup"><span data-stu-id="59722-379">The following example shows an **EntityContainerMapping** element that maps the **SchoolModelEntities** container (the conceptual model entity container) to the **SchoolModelStoreContainer** container (the storage model entity container):</span></span>

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolModelEntities">
   <EntitySetMapping Name="Courses">
     <EntityTypeMapping TypeName="c.Course">
       <MappingFragment StoreEntitySet="Course">
         <ScalarProperty Name="CourseID" ColumnName="CourseID" />
         <ScalarProperty Name="Title" ColumnName="Title" />
         <ScalarProperty Name="Credits" ColumnName="Credits" />
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments">
     <EntityTypeMapping TypeName="c.Department">
       <MappingFragment StoreEntitySet="Department">
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         <ScalarProperty Name="Name" ColumnName="Name" />
         <ScalarProperty Name="Budget" ColumnName="Budget" />
         <ScalarProperty Name="StartDate" ColumnName="StartDate" />
         <ScalarProperty Name="Administrator" ColumnName="Administrator" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
 </EntityContainerMapping>
```

## <a name="entitysetmapping-element-msl"></a><span data-ttu-id="59722-380">EntitySetMapping-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="59722-380">EntitySetMapping Element (MSL)</span></span>

<span data-ttu-id="59722-381">Das **EntitySetMapping** -Element in der Mapping-Spezifikationssprache (MSL) ordnet alle Typen in einer Entitätenmenge eines konzeptionellen Modells Entitätenmengen im Speichermodell zu.</span><span class="sxs-lookup"><span data-stu-id="59722-381">The **EntitySetMapping** element in mapping specification language (MSL) maps all types in a conceptual model entity set to entity sets in the storage model.</span></span> <span data-ttu-id="59722-382">Eine Entitätenmenge im konzeptionellen Modell ist ein logischer Container für Instanzen der Entitäten des gleichen Typs (und abgeleiteter Typen).</span><span class="sxs-lookup"><span data-stu-id="59722-382">An entity set in the conceptual model is a logical container for instances of entities of the same type (and derived types).</span></span> <span data-ttu-id="59722-383">Eine Entitätenmenge im Speichermodell stellt eine Tabelle oder eine Ansicht in der zugrunde liegenden Datenbank dar.</span><span class="sxs-lookup"><span data-stu-id="59722-383">An entity set in the storage model represents a table or view in the underlying database.</span></span> <span data-ttu-id="59722-384">Die Entitätenmenge des konzeptionellen Modells wird durch den Wert des **Name** -Attributs des **EntitySetMapping** -Elements angegeben.</span><span class="sxs-lookup"><span data-stu-id="59722-384">The conceptual model entity set is specified by the value of the **Name** attribute of the **EntitySetMapping** element.</span></span> <span data-ttu-id="59722-385">Die Tabelle oder Sicht, die zugeordnet ist, wird durch das **StoreEntitySet** -Attribut in jedem untergeordneten MappingFragment-Element oder im **EntitySetMapping** -Element selbst angegeben.</span><span class="sxs-lookup"><span data-stu-id="59722-385">The mapped-to table or view is specified by the **StoreEntitySet** attribute in each child MappingFragment element or in the **EntitySetMapping** element itself.</span></span>

<span data-ttu-id="59722-386">Das **EntitySetMapping** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="59722-386">The **EntitySetMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="59722-387">EntityTypeMapping (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-387">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="59722-388">QueryView (null oder eins)</span><span class="sxs-lookup"><span data-stu-id="59722-388">QueryView (zero or one)</span></span>
-   <span data-ttu-id="59722-389">MappingFragment (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-389">MappingFragment (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="59722-390">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="59722-390">Applicable Attributes</span></span>

<span data-ttu-id="59722-391">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **EntitySetMapping** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="59722-391">The following table describes the attributes that can be applied to the **EntitySetMapping** element.</span></span>

| <span data-ttu-id="59722-392">Attributname</span><span class="sxs-lookup"><span data-stu-id="59722-392">Attribute Name</span></span>           | <span data-ttu-id="59722-393">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="59722-393">Is Required</span></span> | <span data-ttu-id="59722-394">value</span><span class="sxs-lookup"><span data-stu-id="59722-394">Value</span></span>                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="59722-395">**Name**</span><span class="sxs-lookup"><span data-stu-id="59722-395">**Name**</span></span>                 | <span data-ttu-id="59722-396">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-396">Yes</span></span>         | <span data-ttu-id="59722-397">Der Name der Entitätenmenge im konzeptionellen Modell, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-397">The name of the conceptual model entity set that is being mapped.</span></span>                                                                                                                                                             |
| <span data-ttu-id="59722-398">**Typname** **1**</span><span class="sxs-lookup"><span data-stu-id="59722-398">**TypeName** **1**</span></span>       | <span data-ttu-id="59722-399">Nein</span><span class="sxs-lookup"><span data-stu-id="59722-399">No</span></span>          | <span data-ttu-id="59722-400">Der Name des Entitätstyp im konzeptionellen Modell, der zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-400">The name of the conceptual model entity type that is being mapped.</span></span>                                                                                                                                                            |
| <span data-ttu-id="59722-401">**StoreEntitySet** **1**</span><span class="sxs-lookup"><span data-stu-id="59722-401">**StoreEntitySet** **1**</span></span> | <span data-ttu-id="59722-402">Nein</span><span class="sxs-lookup"><span data-stu-id="59722-402">No</span></span>          | <span data-ttu-id="59722-403">Der Name der Entitätenmenge im Speichermodell, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-403">The name of the storage model entity set that is being mapped to.</span></span>                                                                                                                                                             |
| <span data-ttu-id="59722-404">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="59722-404">**MakeColumnsDistinct**</span></span>  | <span data-ttu-id="59722-405">Nein</span><span class="sxs-lookup"><span data-stu-id="59722-405">No</span></span>          | <span data-ttu-id="59722-406">**True** oder **false** , abhängig davon, ob nur unterschiedliche Zeilen zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="59722-406">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="59722-407">Wenn dieses Attribut auf **true**festgelegt ist, muss das **generateupdateviews** -Attribut des EntityContainerMapping-Elements auf **false**festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="59722-407">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

 

<span data-ttu-id="59722-408">**1** das **tykame** -Attribut und das **StoreEntitySet** -Attribut können anstelle der untergeordneten EntityTypeMapping-und MappingFragment-Elemente verwendet werden, um einen einzelnen Entitätstyp einer einzelnen Tabelle zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="59722-408">**1** The **TypeName** and **StoreEntitySet** attributes can be used in place of the EntityTypeMapping and MappingFragment child elements to map a single entity type to a single table.</span></span>

### <a name="example"></a><span data-ttu-id="59722-409">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-409">Example</span></span>

<span data-ttu-id="59722-410">Das folgende Beispiel zeigt ein **EntitySetMapping** -Element, das drei Typen (einen Basistyp und zwei abgeleitete Typen) in **der Entitätenmenge des konzeptionellen** Modells drei verschiedenen Tabellen in der zugrunde liegenden Datenbank zuordnet.</span><span class="sxs-lookup"><span data-stu-id="59722-410">The following example shows an **EntitySetMapping** element that maps three types (a base type and two derived types) in the **Courses** entity set of the conceptual model to three different tables in the underlying database.</span></span> <span data-ttu-id="59722-411">Die Tabellen werden durch das **StoreEntitySet** -Attribut in jedem **MappingFragment** -Element angegeben.</span><span class="sxs-lookup"><span data-stu-id="59722-411">The tables are specified by the **StoreEntitySet** attribute in each **MappingFragment** element.</span></span>

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.Course)">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnlineCourse)">
     <MappingFragment StoreEntitySet="OnlineCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="URL" ColumnName="URL" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnsiteCourse)">
     <MappingFragment StoreEntitySet="OnsiteCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Time" ColumnName="Time" />
       <ScalarProperty Name="Days" ColumnName="Days" />
       <ScalarProperty Name="Location" ColumnName="Location" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="entitytypemapping-element-msl"></a><span data-ttu-id="59722-412">EntityTypeMapping-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="59722-412">EntityTypeMapping Element (MSL)</span></span>

<span data-ttu-id="59722-413">Das **EntityTypeMapping** -Element in der Mapping-Spezifikationssprache (Mapping Specification Language, MSL) definiert die Zuordnung zwischen einem Entitätstyp im konzeptionellen Modell und den Tabellen oder Sichten in der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="59722-413">The **EntityTypeMapping** element in mapping specification language (MSL) defines the mapping between an entity type in the conceptual model and tables or views in the underlying database.</span></span> <span data-ttu-id="59722-414">Informationen zu den Entitätstypen des konzeptionellen Modells und den zugrunde liegenden Datenbanktabellen oder Ansichten finden Sie in EntityType-Element (CSDL) und EntitySet-Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="59722-414">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="59722-415">Der Entitätstyp des konzeptionellen Modells, der zugeordnet wird, wird durch das **tykame** -Attribut des **EntityTypeMapping** -Elements angegeben.</span><span class="sxs-lookup"><span data-stu-id="59722-415">The conceptual model entity type that is being mapped is specified by the **TypeName** attribute of the **EntityTypeMapping** element.</span></span> <span data-ttu-id="59722-416">Die Tabelle oder Sicht, die zugeordnet wird, wird durch das **StoreEntitySet** -Attribut des untergeordneten MappingFragment-Elements angegeben.</span><span class="sxs-lookup"><span data-stu-id="59722-416">The table or view that is being mapped is specified by the **StoreEntitySet** attribute of the child MappingFragment element.</span></span>

<span data-ttu-id="59722-417">Das untergeordnete ModificationFunctionMapping-Element kann verwendet werden, um gespeicherten Prozeduren in der Datenbank die Einfüge-, Aktualisierungs- oder Löschfunktionen von Entitätstypen zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="59722-417">The ModificationFunctionMapping child element can be used to map the insert, update, or delete functions of entity types to stored procedures in the database.</span></span>

<span data-ttu-id="59722-418">Das **EntityTypeMapping** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="59722-418">The **EntityTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="59722-419">MappingFragment (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-419">MappingFragment (zero or more)</span></span>
-   <span data-ttu-id="59722-420">ModificationFunctionMapping (0 (null) oder 1)</span><span class="sxs-lookup"><span data-stu-id="59722-420">ModificationFunctionMapping (zero or one)</span></span>
-   <span data-ttu-id="59722-421">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="59722-421">ScalarProperty</span></span>
-   <span data-ttu-id="59722-422">Bedingung</span><span class="sxs-lookup"><span data-stu-id="59722-422">Condition</span></span>

> [!NOTE]
> <span data-ttu-id="59722-423">**MappingFragment** -und **ModificationFunctionMapping** -Elemente können nicht gleichzeitig untergeordnete Elemente des **EntityTypeMapping** -Elements sein.</span><span class="sxs-lookup"><span data-stu-id="59722-423">**MappingFragment** and **ModificationFunctionMapping** elements cannot be child elements of the **EntityTypeMapping** element at the same time.</span></span>


> [!NOTE]
> <span data-ttu-id="59722-424">Die **ScalarProperty** -und **Condition** -Elemente können nur untergeordnete Elemente des **EntityTypeMapping** -Elements sein, wenn Sie in einem FunctionImportMapping-Element verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="59722-424">The **ScalarProperty** and **Condition** elements can only be child elements of the **EntityTypeMapping** element when it is used within a FunctionImportMapping element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="59722-425">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="59722-425">Applicable Attributes</span></span>

<span data-ttu-id="59722-426">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **EntityTypeMapping** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="59722-426">The following table describes the attributes that can be applied to the **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="59722-427">Attributname</span><span class="sxs-lookup"><span data-stu-id="59722-427">Attribute Name</span></span> | <span data-ttu-id="59722-428">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="59722-428">Is Required</span></span> | <span data-ttu-id="59722-429">value</span><span class="sxs-lookup"><span data-stu-id="59722-429">Value</span></span>                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="59722-430">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="59722-430">**TypeName**</span></span>   | <span data-ttu-id="59722-431">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-431">Yes</span></span>         | <span data-ttu-id="59722-432">Der mit einem Namespace qualifizierte Name des Entitätstyps des konzeptionellen Modells, der zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-432">The namespace-qualified name of the conceptual model entity type that is being mapped.</span></span> <br/> <span data-ttu-id="59722-433">Wenn der Typ abstrakt oder ein abgeleiteter Typ ist, muss der Wert `IsOfType(Namespace-qualified_type_name)` lauten.</span><span class="sxs-lookup"><span data-stu-id="59722-433">If the type is abstract or a derived type, the value must be `IsOfType(Namespace-qualified_type_name)`.</span></span> |

### <a name="example"></a><span data-ttu-id="59722-434">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-434">Example</span></span>

<span data-ttu-id="59722-435">Das folgende Beispiel zeigt ein EntitySetMapping-Element mit zwei untergeordneten **EntityTypeMapping** -Elementen.</span><span class="sxs-lookup"><span data-stu-id="59722-435">The following example shows an EntitySetMapping element with two child **EntityTypeMapping** elements.</span></span> <span data-ttu-id="59722-436">Im ersten **EntityTypeMapping** -Element wird der " **School Model. Person** "-Entitätstyp der **Person** -Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="59722-436">In the first **EntityTypeMapping** element, the **SchoolModel.Person** entity type is mapped to the **Person** table.</span></span> <span data-ttu-id="59722-437">Im zweiten **EntityTypeMapping** -Element wird die Aktualisierungs Funktion des **SchoolModel. Person** -Typs der gespeicherten Prozedur **updateperson**in der-Datenbank zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="59722-437">In the second **EntityTypeMapping** element, the update functionality of the **SchoolModel.Person** type is mapped to a stored procedure, **UpdatePerson**, in the database.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate" ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="59722-438">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-438">Example</span></span>

<span data-ttu-id="59722-439">Im nächsten Beispiel wird die Zuordnung einer Typhierarchie, in der der Stammtyp abstrakt ist, veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="59722-439">The next example shows the mapping of a type hierarchy in which the root type is abstract.</span></span> <span data-ttu-id="59722-440">Beachten Sie die Verwendung der `IsOfType`-Syntax für die **tykename** -Attribute.</span><span class="sxs-lookup"><span data-stu-id="59722-440">Note the use of the `IsOfType` syntax for the **TypeName** attributes.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="functionimportmapping-element-msl"></a><span data-ttu-id="59722-441">FunctionImportMapping-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="59722-441">FunctionImportMapping Element (MSL)</span></span>

<span data-ttu-id="59722-442">Das **FunctionImportMapping** -Element in der Mapping-Spezifikationssprache (Mapping Specification Language, MSL) definiert die Zuordnung zwischen einem Funktions Import im konzeptionellen Modell und einer gespeicherten Prozedur oder Funktion in der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="59722-442">The **FunctionImportMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure or function in the underlying database.</span></span> <span data-ttu-id="59722-443">Funktionsimporte müssen im konzeptionellen Modell und gespeicherte Prozeduren müssen im Speichermodell deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="59722-443">Function imports must be declared in the conceptual model and stored procedures must be declared in the storage model.</span></span> <span data-ttu-id="59722-444">Weitere Informationen finden Sie unter FunctionImport-Element (CSDL) und Function-Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="59722-444">For more information, see FunctionImport Element (CSDL) and Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="59722-445">Wenn ein Funktionsimport einen Entitätstyp des konzeptionellen Modells oder einen komplexen Typ zurückgibt, dann müssen standardmäßig die Namen der Spalten, die von der zugrunde liegenden gespeicherten Prozedur zurückgegeben werden, exakt den Namen der Eigenschaften des konzeptionellen Modelltyps entsprechen.</span><span class="sxs-lookup"><span data-stu-id="59722-445">By default, if a function import returns a conceptual model entity type or complex type, then the names of the columns returned by the underlying stored procedure must exactly match the names of the properties on the conceptual model type.</span></span> <span data-ttu-id="59722-446">Wenn die Spaltennamen nicht exakt mit den Eigenschaftsnamen übereinstimmen, muss die Zuordnung in einem resultmapping-Element definiert werden.</span><span class="sxs-lookup"><span data-stu-id="59722-446">If the column names do not exactly match the property names, the mapping must be defined in a ResultMapping element.</span></span>

<span data-ttu-id="59722-447">Das **FunctionImportMapping** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="59722-447">The **FunctionImportMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="59722-448">Resultmapping (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-448">ResultMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="59722-449">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="59722-449">Applicable Attributes</span></span>

<span data-ttu-id="59722-450">In der folgenden Tabelle werden die Attribute beschrieben, die für das **FunctionImportMapping** -Element anwendbar sind:</span><span class="sxs-lookup"><span data-stu-id="59722-450">The following table describes the attributes that are applicable to the **FunctionImportMapping** element:</span></span>

| <span data-ttu-id="59722-451">Attributname</span><span class="sxs-lookup"><span data-stu-id="59722-451">Attribute Name</span></span>         | <span data-ttu-id="59722-452">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="59722-452">Is Required</span></span> | <span data-ttu-id="59722-453">value</span><span class="sxs-lookup"><span data-stu-id="59722-453">Value</span></span>                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| <span data-ttu-id="59722-454">**FunctionImportName**</span><span class="sxs-lookup"><span data-stu-id="59722-454">**FunctionImportName**</span></span> | <span data-ttu-id="59722-455">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-455">Yes</span></span>         | <span data-ttu-id="59722-456">Der Name des Funktionsimports im konzeptionellen Modell, der zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-456">The name of the function import in the conceptual model that is being mapped.</span></span>           |
| <span data-ttu-id="59722-457">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="59722-457">**FunctionName**</span></span>       | <span data-ttu-id="59722-458">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-458">Yes</span></span>         | <span data-ttu-id="59722-459">Der mit einem Namespace qualifizierte Name der Funktion im Speichermodell, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-459">The namespace-qualified name of the function in the storage model that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="59722-460">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-460">Example</span></span>

<span data-ttu-id="59722-461">Das folgende Beispiel beruht auf dem School-Modell.</span><span class="sxs-lookup"><span data-stu-id="59722-461">The following example is based on the School model.</span></span> <span data-ttu-id="59722-462">Betrachten Sie die folgende Funktion im Speichermodell:</span><span class="sxs-lookup"><span data-stu-id="59722-462">Consider the following function in the storage model:</span></span>

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

<span data-ttu-id="59722-463">Beachten Sie auch diesen Funktionsimport im konzeptionellen Modell:</span><span class="sxs-lookup"><span data-stu-id="59722-463">Also consider this function import in the conceptual model:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

<span data-ttu-id="59722-464">Das folgende Beispiel zeigt ein **FunctionImportMapping** -Element, das verwendet wird, um den Funktions-und Funktions Import einander zuzuordnen:</span><span class="sxs-lookup"><span data-stu-id="59722-464">The following example show a **FunctionImportMapping** element used to map the function and function import above to each other:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a><span data-ttu-id="59722-465">InsertFunction-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="59722-465">InsertFunction Element (MSL)</span></span>

<span data-ttu-id="59722-466">Das **InsertFunction** -Element in der Mapping-Spezifikationssprache (Mapping Specification Language, MSL) ordnet die INSERT-Funktion eines Entitäts Typs oder einer Zuordnung im konzeptionellen Modell einer gespeicherten Prozedur in der zugrunde liegenden Datenbank zu.</span><span class="sxs-lookup"><span data-stu-id="59722-466">The **InsertFunction** element in mapping specification language (MSL) maps the insert function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="59722-467">Gespeicherte Prozeduren, denen Änderungsfunktionen zugeordnet werden, müssen im Speichermodell deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="59722-467">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="59722-468">Weitere Informationen finden Sie unter Function-Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="59722-468">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="59722-469">Wenn Sie nicht alle drei Einfüge-, Aktualisierungs-oder Löschvorgänge eines Entitäts Typs gespeicherten Prozeduren zuordnen, schlagen die nicht zugeordneten Vorgänge fehl, wenn Sie zur Laufzeit ausgeführt werden und eine Update Exception ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="59722-469">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="59722-470">Das **InsertFunction** -Element kann ein untergeordnetes Element des ModificationFunctionMapping-Elements sein und auf das EntityTypeMapping-Element oder das AssociationSetMapping-Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="59722-470">The **InsertFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

### <a name="insertfunction-applied-to-entitytypemapping"></a><span data-ttu-id="59722-471">InsertFunction angewendet auf EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="59722-471">InsertFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="59722-472">Wenn das **InsertFunction** -Element auf das EntityTypeMapping-Element angewendet wird, ordnet es die INSERT-Funktion eines Entitäts Typs im konzeptionellen Modell einer gespeicherten Prozedur zu.</span><span class="sxs-lookup"><span data-stu-id="59722-472">When applied to the EntityTypeMapping element, the **InsertFunction** element maps the insert function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="59722-473">Das **InsertFunction** -Element kann die folgenden untergeordneten Elemente aufweisen, wenn es auf ein **EntityTypeMapping** -Element angewendet wird:</span><span class="sxs-lookup"><span data-stu-id="59722-473">The **InsertFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="59722-474">AssociationEnd (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-474">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="59722-475">ComplexProperty (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-475">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="59722-476">ResultBinding (null oder 1)</span><span class="sxs-lookup"><span data-stu-id="59722-476">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="59722-477">Scarlarproperty (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-477">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="59722-478">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="59722-478">Applicable Attributes</span></span>

<span data-ttu-id="59722-479">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **InsertFunction** -Element angewendet werden können, wenn es auf ein **EntityTypeMapping** -Element angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-479">The following table describes the attributes that can be applied to the **InsertFunction** element when applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="59722-480">Attributname</span><span class="sxs-lookup"><span data-stu-id="59722-480">Attribute Name</span></span>            | <span data-ttu-id="59722-481">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="59722-481">Is Required</span></span> | <span data-ttu-id="59722-482">value</span><span class="sxs-lookup"><span data-stu-id="59722-482">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="59722-483">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="59722-483">**FunctionName**</span></span>          | <span data-ttu-id="59722-484">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-484">Yes</span></span>         | <span data-ttu-id="59722-485">Der mit einem Namespace qualifizierte Name der gespeicherten Prozedur, der die Einfügefunktion zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-485">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="59722-486">Die gespeicherte Prozedur muss im Speichermodell deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="59722-486">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="59722-487">**Rowsaffectedparameter**</span><span class="sxs-lookup"><span data-stu-id="59722-487">**RowsAffectedParameter**</span></span> | <span data-ttu-id="59722-488">Nein</span><span class="sxs-lookup"><span data-stu-id="59722-488">No</span></span>          | <span data-ttu-id="59722-489">Der Name des Ausgabeparameters, der die Anzahl der betroffenen Zeilen zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="59722-489">The name of the output parameter that returns the number of affected rows.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="59722-490">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-490">Example</span></span>

<span data-ttu-id="59722-491">Das folgende Beispiel basiert auf dem Modell "School" und zeigt das **InsertFunction** -Element, das verwendet wird, um die Einfügefunktion des Entitäts Typs "Person" der gespeicherten Prozedur **InsertPerson** zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="59722-491">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the Person entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="59722-492">Die gespeicherte Prozedur **InsertPerson** wird im Speichermodell deklariert.</span><span class="sxs-lookup"><span data-stu-id="59722-492">The **InsertPerson** stored procedure is declared in the storage model.</span></span>

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
### <a name="insertfunction-applied-to-associationsetmapping"></a><span data-ttu-id="59722-493">InsertFunction angewendet auf AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="59722-493">InsertFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="59722-494">Wenn das **InsertFunction** -Element auf das AssociationSetMapping-Element angewendet wird, ordnet es die INSERT-Funktion einer Zuordnung im konzeptionellen Modell einer gespeicherten Prozedur zu.</span><span class="sxs-lookup"><span data-stu-id="59722-494">When applied to the AssociationSetMapping element, the **InsertFunction** element maps the insert function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="59722-495">Das **InsertFunction** -Element kann die folgenden untergeordneten Elemente aufweisen, wenn es auf das **AssociationSetMapping** -Element angewendet wird:</span><span class="sxs-lookup"><span data-stu-id="59722-495">The **InsertFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="59722-496">EndProperty</span><span class="sxs-lookup"><span data-stu-id="59722-496">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="59722-497">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="59722-497">Applicable Attributes</span></span>

<span data-ttu-id="59722-498">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **InsertFunction** -Element angewendet werden können, wenn es auf das **AssociationSetMapping** -Element angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-498">The following table describes the attributes that can be applied to the **InsertFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="59722-499">Attributname</span><span class="sxs-lookup"><span data-stu-id="59722-499">Attribute Name</span></span>            | <span data-ttu-id="59722-500">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="59722-500">Is Required</span></span> | <span data-ttu-id="59722-501">value</span><span class="sxs-lookup"><span data-stu-id="59722-501">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="59722-502">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="59722-502">**FunctionName**</span></span>          | <span data-ttu-id="59722-503">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-503">Yes</span></span>         | <span data-ttu-id="59722-504">Der mit einem Namespace qualifizierte Name der gespeicherten Prozedur, der die Einfügefunktion zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-504">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="59722-505">Die gespeicherte Prozedur muss im Speichermodell deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="59722-505">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="59722-506">**Rowsaffectedparameter**</span><span class="sxs-lookup"><span data-stu-id="59722-506">**RowsAffectedParameter**</span></span> | <span data-ttu-id="59722-507">Nein</span><span class="sxs-lookup"><span data-stu-id="59722-507">No</span></span>          | <span data-ttu-id="59722-508">Der Name des Ausgabeparameters, der die Anzahl der betroffenen Zeilen zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="59722-508">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="59722-509">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-509">Example</span></span>

<span data-ttu-id="59722-510">Das folgende Beispiel basiert auf dem Modell "School" und zeigt das **InsertFunction** -Element, das verwendet wird, um die INSERT-Funktion der " **CourseInstructor** "-Zuordnung der gespeicherten Prozedur **insertcourseinstructor** zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="59722-510">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the **CourseInstructor** association to the **InsertCourseInstructor** stored procedure.</span></span> <span data-ttu-id="59722-511">Die gespeicherte Prozedur **insertcourseinstructor** ist im Speichermodell deklariert.</span><span class="sxs-lookup"><span data-stu-id="59722-511">The **InsertCourseInstructor** stored procedure is declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="mapping-element-msl"></a><span data-ttu-id="59722-512">Mapping-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="59722-512">Mapping Element (MSL)</span></span>

<span data-ttu-id="59722-513">Das **Mapping** -Element in der Mapping-Spezifikationssprache (Mapping Specification Language, MSL) enthält Informationen zum Mapping von Objekten, die in einem konzeptionellen Modell definiert sind, zu einer Datenbank (wie in einem Speichermodell beschrieben).</span><span class="sxs-lookup"><span data-stu-id="59722-513">The **Mapping** element in mapping specification language (MSL) contains information for mapping objects that are defined in a conceptual model to a database (as described in a storage model).</span></span> <span data-ttu-id="59722-514">Weitere Informationen finden Sie unter CSDL-Spezifikation und SSDL-Spezifikation.</span><span class="sxs-lookup"><span data-stu-id="59722-514">For more information, see CSDL Specification and SSDL Specification.</span></span>

<span data-ttu-id="59722-515">Das **Mapping** -Element ist das Stamm Element für eine Mappingspezifikation.</span><span class="sxs-lookup"><span data-stu-id="59722-515">The **Mapping** element is the root element for a mapping specification.</span></span> <span data-ttu-id="59722-516">Der XML-Namespace für Mappingspezifikationen ist https://schemas.microsoft.com/ado/2009/11/mapping/cs.</span><span class="sxs-lookup"><span data-stu-id="59722-516">The XML namespace for mapping specifications is https://schemas.microsoft.com/ado/2009/11/mapping/cs.</span></span>

<span data-ttu-id="59722-517">Das Mapping-Element kann die folgenden untergeordneten Elemente aufweisen (der vorliegenden Reihenfolge entsprechend):</span><span class="sxs-lookup"><span data-stu-id="59722-517">The mapping element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="59722-518">Alias (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-518">Alias (zero or more)</span></span>
-   <span data-ttu-id="59722-519">EntityContainerMapping (genau 1)</span><span class="sxs-lookup"><span data-stu-id="59722-519">EntityContainerMapping (exactly one)</span></span>

<span data-ttu-id="59722-520">Die Namen aller Typen des konzeptionellen Modells und Typen des Speichermodells, auf die in MSL verwiesen wird, müssen mit dem jeweiligen Namespacenamen qualifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="59722-520">Names of conceptual and storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="59722-521">Weitere Informationen zum Namespace Namen des konzeptionellen Modells finden Sie unter Schema-Element (CSDL).</span><span class="sxs-lookup"><span data-stu-id="59722-521">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="59722-522">Weitere Informationen zum Namespace Namen des Speicher Modells finden Sie unter Schema-Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="59722-522">For information about the storage model namespace name, see Schema Element (SSDL).</span></span> <span data-ttu-id="59722-523">Aliasnamen für Namespaces, die in MSL verwendet werden, können mit dem Alias-Element definiert werden.</span><span class="sxs-lookup"><span data-stu-id="59722-523">Aliases for namespaces that are used in MSL can be defined with the Alias element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="59722-524">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="59722-524">Applicable Attributes</span></span>

<span data-ttu-id="59722-525">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **Mapping** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="59722-525">The table below describes the attributes that can be applied to the **Mapping** element.</span></span>

| <span data-ttu-id="59722-526">Attributname</span><span class="sxs-lookup"><span data-stu-id="59722-526">Attribute Name</span></span> | <span data-ttu-id="59722-527">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="59722-527">Is Required</span></span> | <span data-ttu-id="59722-528">value</span><span class="sxs-lookup"><span data-stu-id="59722-528">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="59722-529">**LEERTASTE**</span><span class="sxs-lookup"><span data-stu-id="59722-529">**Space**</span></span>      | <span data-ttu-id="59722-530">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-530">Yes</span></span>         | <span data-ttu-id="59722-531">**C-S**.</span><span class="sxs-lookup"><span data-stu-id="59722-531">**C-S**.</span></span> <span data-ttu-id="59722-532">Dies ist ein fester Wert, der nicht geändert werden kann.</span><span class="sxs-lookup"><span data-stu-id="59722-532">This is a fixed value and cannot be changed.</span></span> |

### <a name="example"></a><span data-ttu-id="59722-533">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-533">Example</span></span>

<span data-ttu-id="59722-534">Das folgende Beispiel zeigt ein **Mapping** -Element, das auf einem Teil des Modells "School" basiert.</span><span class="sxs-lookup"><span data-stu-id="59722-534">The following example shows a **Mapping** element that is based on part of the School model.</span></span> <span data-ttu-id="59722-535">Weitere Informationen zum Modell "School" finden Sie unter Schnellstart (Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="59722-535">For more information about the School model, see Quickstart (Entity Framework):</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="https://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="mappingfragment-element-msl"></a><span data-ttu-id="59722-536">MappingFragment-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="59722-536">MappingFragment Element (MSL)</span></span>

<span data-ttu-id="59722-537">Das **MappingFragment** -Element in der Mapping-Spezifikationssprache (Mapping Specification Language, MSL) definiert die Zuordnung zwischen den Eigenschaften eines Entitäts Typs des konzeptionellen Modells und einer Tabelle oder Sicht in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="59722-537">The **MappingFragment** element in mapping specification language (MSL) defines the mapping between the properties of a conceptual model entity type and a table or view in the database.</span></span> <span data-ttu-id="59722-538">Informationen zu den Entitätstypen des konzeptionellen Modells und den zugrunde liegenden Datenbanktabellen oder Ansichten finden Sie in EntityType-Element (CSDL) und EntitySet-Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="59722-538">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="59722-539">Das **MappingFragment** kann ein untergeordnetes Element des EntityTypeMapping-Elements oder des EntitySetMapping-Elements sein.</span><span class="sxs-lookup"><span data-stu-id="59722-539">The **MappingFragment** can be a child element of the EntityTypeMapping element or the EntitySetMapping element.</span></span>

<span data-ttu-id="59722-540">Das **MappingFragment** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="59722-540">The **MappingFragment** element can have the following child elements:</span></span>

-   <span data-ttu-id="59722-541">ComplexType (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-541">ComplexType (zero or more)</span></span>
-   <span data-ttu-id="59722-542">ScalarProperty (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-542">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="59722-543">Bedingung (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-543">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="59722-544">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="59722-544">Applicable Attributes</span></span>

<span data-ttu-id="59722-545">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **MappingFragment** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="59722-545">The following table describes the attributes that can be applied to the **MappingFragment** element.</span></span>

| <span data-ttu-id="59722-546">Attributname</span><span class="sxs-lookup"><span data-stu-id="59722-546">Attribute Name</span></span>          | <span data-ttu-id="59722-547">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="59722-547">Is Required</span></span> | <span data-ttu-id="59722-548">value</span><span class="sxs-lookup"><span data-stu-id="59722-548">Value</span></span>                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="59722-549">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="59722-549">**StoreEntitySet**</span></span>      | <span data-ttu-id="59722-550">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-550">Yes</span></span>         | <span data-ttu-id="59722-551">Der Name der Tabelle oder Ansicht, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-551">The name of the table or view that is being mapped.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="59722-552">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="59722-552">**MakeColumnsDistinct**</span></span> | <span data-ttu-id="59722-553">Nein</span><span class="sxs-lookup"><span data-stu-id="59722-553">No</span></span>          | <span data-ttu-id="59722-554">**True** oder **false** , abhängig davon, ob nur unterschiedliche Zeilen zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="59722-554">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="59722-555">Wenn dieses Attribut auf **true**festgelegt ist, muss das **generateupdateviews** -Attribut des EntityContainerMapping-Elements auf **false**festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="59722-555">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

### <a name="example"></a><span data-ttu-id="59722-556">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-556">Example</span></span>

<span data-ttu-id="59722-557">Das folgende Beispiel zeigt ein **MappingFragment** -Element als untergeordnetes Element eines **EntityTypeMapping** -Elements.</span><span class="sxs-lookup"><span data-stu-id="59722-557">The following example shows a **MappingFragment** element as the child of an **EntityTypeMapping** element.</span></span> <span data-ttu-id="59722-558">In diesem Beispiel werden Eigenschaften des **Course** -Typs im konzeptionellen Modell den Spalten der **Course** -Tabelle in der-Datenbank zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="59722-558">In this example, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="59722-559">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-559">Example</span></span>

<span data-ttu-id="59722-560">Das folgende Beispiel zeigt ein **MappingFragment** -Element als untergeordnetes Element eines **EntitySetMapping** -Elements.</span><span class="sxs-lookup"><span data-stu-id="59722-560">The following example shows a **MappingFragment** element as the child of an **EntitySetMapping** element.</span></span> <span data-ttu-id="59722-561">Wie im obigen Beispiel werden Eigenschaften des **Course** -Typs im konzeptionellen Modell den Spalten der **Course** -Tabelle in der-Datenbank zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="59722-561">As in the example above, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

``` xml
 <EntitySetMapping Name="Courses" TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
 </EntitySetMapping>
```

## <a name="modificationfunctionmapping-element-msl"></a><span data-ttu-id="59722-562">ModificationFunctionMapping-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="59722-562">ModificationFunctionMapping Element (MSL)</span></span>

<span data-ttu-id="59722-563">Das **ModificationFunctionMapping** -Element in der Mapping-Spezifikationssprache (MSL) ordnet die INSERT-, Update-und DELETE-Funktionen eines Entitäts Typs des konzeptionellen Modells gespeicherten Prozeduren in der zugrunde liegenden Datenbank zu.</span><span class="sxs-lookup"><span data-stu-id="59722-563">The **ModificationFunctionMapping** element in mapping specification language (MSL) maps the insert, update, and delete functions of a conceptual model entity type to stored procedures in the underlying database.</span></span> <span data-ttu-id="59722-564">Das **ModificationFunctionMapping** -Element kann auch die INSERT-und DELETE-Funktionen für m:n-Zuordnungen im konzeptionellen Modell zu gespeicherten Prozeduren in der zugrunde liegenden Datenbank zuordnen.</span><span class="sxs-lookup"><span data-stu-id="59722-564">The **ModificationFunctionMapping** element can also map the insert and delete functions for many-to-many associations in the conceptual model to stored procedures in the underlying database.</span></span> <span data-ttu-id="59722-565">Gespeicherte Prozeduren, denen Änderungsfunktionen zugeordnet werden, müssen im Speichermodell deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="59722-565">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="59722-566">Weitere Informationen finden Sie unter Function-Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="59722-566">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="59722-567">Wenn Sie nicht alle drei Einfüge-, Aktualisierungs-oder Löschvorgänge eines Entitäts Typs gespeicherten Prozeduren zuordnen, schlagen die nicht zugeordneten Vorgänge fehl, wenn Sie zur Laufzeit ausgeführt werden und eine Update Exception ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="59722-567">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>


> [!NOTE]
> <span data-ttu-id="59722-568">Wenn die Änderungsfunktionen für eine Entität in einer Vererbungshierarchie gespeicherten Prozeduren zugeordnet werden, dann müssen die Änderungsfunktionen für alle Typen in der Hierarchie gespeicherten Prozeduren zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="59722-568">If the modification functions for one entity in an inheritance hierarchy are mapped to stored procedures, then modification functions for all types in the hierarchy must be mapped to stored procedures.</span></span>

<span data-ttu-id="59722-569">Das **ModificationFunctionMapping** -Element kann ein untergeordnetes Element des EntityTypeMapping-Elements oder des AssociationSetMapping-Elements sein.</span><span class="sxs-lookup"><span data-stu-id="59722-569">The **ModificationFunctionMapping** element can be a child of the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

<span data-ttu-id="59722-570">Das **ModificationFunctionMapping** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="59722-570">The **ModificationFunctionMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="59722-571">DeleteFunction (null oder 1)</span><span class="sxs-lookup"><span data-stu-id="59722-571">DeleteFunction (zero or one)</span></span>
-   <span data-ttu-id="59722-572">InsertFunction (0 (null) oder 1)</span><span class="sxs-lookup"><span data-stu-id="59722-572">InsertFunction (zero or one)</span></span>
-   <span data-ttu-id="59722-573">UpdateFunction (null oder 1)</span><span class="sxs-lookup"><span data-stu-id="59722-573">UpdateFunction (zero or one)</span></span>

<span data-ttu-id="59722-574">Für das **ModificationFunctionMapping** -Element sind keine Attribute anwendbar.</span><span class="sxs-lookup"><span data-stu-id="59722-574">No attributes are applicable to the **ModificationFunctionMapping** element.</span></span>

### <a name="example"></a><span data-ttu-id="59722-575">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-575">Example</span></span>

<span data-ttu-id="59722-576">Das folgende Beispiel zeigt die entitätenmengenzuordnung für die **People** -Entitätenmenge im Modell "School".</span><span class="sxs-lookup"><span data-stu-id="59722-576">The following example shows the entity set mapping for the **People** entity set in the School model.</span></span> <span data-ttu-id="59722-577">Zusätzlich zur Spalten Zuordnung für den Entitätstyp **Person** wird die Zuordnung der INSERT-, Update-und DELETE-Funktionen des **Person** -Typs angezeigt.</span><span class="sxs-lookup"><span data-stu-id="59722-577">In addition to the column mapping for the **Person** entity type, the mapping of the insert, update, and delete functions of the **Person** type are shown.</span></span> <span data-ttu-id="59722-578">Die Funktionen, denen sie zugeordnet werden, sind im Speichermodell deklariert.</span><span class="sxs-lookup"><span data-stu-id="59722-578">The functions that are mapped to are declared in the storage model.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="59722-579">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-579">Example</span></span>

<span data-ttu-id="59722-580">Das folgende Beispiel zeigt die Zuordnung der Zuordnungs Sätze für den im Modell "School" festgelegten **CourseInstructor** -Zuordnungs Satz.</span><span class="sxs-lookup"><span data-stu-id="59722-580">The following example shows the association set mapping for the **CourseInstructor** association set in the School model.</span></span> <span data-ttu-id="59722-581">Zusätzlich zur Spalten Zuordnung für die " **CourseInstructor** "-Zuordnung wird die Zuordnung der INSERT-und DELETE-Funktionen der " **CourseInstructor** "-Zuordnung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="59722-581">In addition to the column mapping for the **CourseInstructor** association, the mapping of the insert and delete functions of the **CourseInstructor** association are shown.</span></span> <span data-ttu-id="59722-582">Die Funktionen, denen sie zugeordnet werden, sind im Speichermodell deklariert.</span><span class="sxs-lookup"><span data-stu-id="59722-582">The functions that are mapped to are declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```
 

 

## <a name="queryview-element-msl"></a><span data-ttu-id="59722-583">QueryView-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="59722-583">QueryView Element (MSL)</span></span>

<span data-ttu-id="59722-584">Das **QueryView** -Element in der Mapping-Spezifikationssprache (Mapping Specification Language, MSL) definiert eine schreibgeschützte Zuordnung zwischen einem Entitätstyp oder einer Zuordnung im konzeptionellen Modell und einer Tabelle in der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="59722-584">The **QueryView** element in mapping specification language (MSL) defines a read-only mapping between an entity type or association in the conceptual model and a table in the underlying database.</span></span> <span data-ttu-id="59722-585">Die Zuordnung wird mit einer Entity SQL Abfrage definiert, die im Speichermodell ausgewertet wird, und Sie können das Resultset in Bezug auf eine Entität oder eine Zuordnung im konzeptionellen Modell Ausdrücken.</span><span class="sxs-lookup"><span data-stu-id="59722-585">The mapping is defined with an Entity SQL query that is evaluated against the storage model, and you express the result set in terms of an entity or association in the conceptual model.</span></span> <span data-ttu-id="59722-586">Da Abfragesichten schreibgeschützt sind, können die durch Abfragesichten definierten Typen nicht mit herkömmlichen Aktualisierungsbefehlen aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="59722-586">Because query views are read-only, you cannot use standard update commands to update types that are defined by query views.</span></span> <span data-ttu-id="59722-587">Diese Typen können mithilfe von Änderungsfunktionen aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="59722-587">You can make updates to these types by using modification functions.</span></span> <span data-ttu-id="59722-588">Weitere Informationen finden Sie unter Gewusst wie: Zuordnen von Änderungs Funktionen zu gespeicherten Prozeduren.</span><span class="sxs-lookup"><span data-stu-id="59722-588">For more information, see How to: Map Modification Functions to Stored Procedures.</span></span>

> [!NOTE]
> <span data-ttu-id="59722-589">Im **QueryView** -Element werden Entity SQL Ausdrücke, die **GroupBy**, Gruppen Aggregate oder Navigations Eigenschaften enthalten, nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="59722-589">In the **QueryView** element, Entity SQL expressions that contain **GroupBy**, group aggregates, or navigation properties are not supported.</span></span>

 

<span data-ttu-id="59722-590">Das **QueryView** -Element kann ein untergeordnetes Element des EntitySetMapping-Elements oder des AssociationSetMapping-Elements sein.</span><span class="sxs-lookup"><span data-stu-id="59722-590">The **QueryView** element can be a child of the EntitySetMapping element or the AssociationSetMapping element.</span></span> <span data-ttu-id="59722-591">Im ersten Fall definiert die Abfrageansicht eine schreibgeschützte Zuordnung für eine Entität im konzeptionellen Modell.</span><span class="sxs-lookup"><span data-stu-id="59722-591">In the former case, the query view defines a read-only mapping for an entity in the conceptual model.</span></span> <span data-ttu-id="59722-592">Im zweiten Fall definiert die Abfrageansicht eine schreibgeschützte Zuordnung für eine Zuordnung im konzeptionellen Modell.</span><span class="sxs-lookup"><span data-stu-id="59722-592">In the latter case, the query view defines a read-only mapping for an association in the conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="59722-593">Wenn das **AssociationSetMapping** -Element für eine Zuordnung zu einer referenziellen Einschränkung vorgesehen ist, wird das **AssociationSetMapping** -Element ignoriert.</span><span class="sxs-lookup"><span data-stu-id="59722-593">If the **AssociationSetMapping** element is for an association with a referential constraint, the **AssociationSetMapping** element is ignored.</span></span> <span data-ttu-id="59722-594">Weitere Informationen finden Sie unter referentialeinschränkungs-Element (CSDL).</span><span class="sxs-lookup"><span data-stu-id="59722-594">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="59722-595">Das **QueryView** -Element darf keine untergeordneten Elemente aufweisen.</span><span class="sxs-lookup"><span data-stu-id="59722-595">The **QueryView** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="59722-596">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="59722-596">Applicable Attributes</span></span>

<span data-ttu-id="59722-597">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **QueryView** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="59722-597">The following table describes the attributes that can be applied to the **QueryView** element.</span></span>

| <span data-ttu-id="59722-598">Attributname</span><span class="sxs-lookup"><span data-stu-id="59722-598">Attribute Name</span></span> | <span data-ttu-id="59722-599">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="59722-599">Is Required</span></span> | <span data-ttu-id="59722-600">value</span><span class="sxs-lookup"><span data-stu-id="59722-600">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="59722-601">**TypeName**</span><span class="sxs-lookup"><span data-stu-id="59722-601">**TypeName**</span></span>   | <span data-ttu-id="59722-602">Nein</span><span class="sxs-lookup"><span data-stu-id="59722-602">No</span></span>          | <span data-ttu-id="59722-603">Der Name des konzeptionellen Modelltyps, der durch die Abfrageansicht zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-603">The name of the conceptual model type that is being mapped by the query view.</span></span> |

### <a name="example"></a><span data-ttu-id="59722-604">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-604">Example</span></span>

<span data-ttu-id="59722-605">Das folgende Beispiel zeigt das **QueryView** -Element als untergeordnetes Element des **EntitySetMapping** -Elements und definiert eine Abfrage Ansichts Zuordnung für den **Department** -Entitätstyp im Modell "School".</span><span class="sxs-lookup"><span data-stu-id="59722-605">The following example shows the **QueryView** element as a child of the **EntitySetMapping** element and defines a query view mapping for the **Department** entity type in the School Model.</span></span>

``` xml
 <EntitySetMapping Name="Departments" >
   <QueryView>
     SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                         d.Name,
                                         d.Budget,
                                         d.StartDate)
     FROM SchoolModelStoreContainer.Department AS d
     WHERE d.Budget > 150000
   </QueryView>
 </EntitySetMapping>
```

<span data-ttu-id="59722-606">Da die Abfrage nur eine Teilmenge der Member des **Abteilungs** Typs im Speichermodell zurückgibt, wurde der **Abteilungs** Typ im Modell "School" wie folgt auf der Grundlage dieser Zuordnung geändert:</span><span class="sxs-lookup"><span data-stu-id="59722-606">Because the query only returns a subset of the members of the **Department** type in the storage model, the **Department** type in the School model has been modified based on this mapping as follows:</span></span>

``` xml
 <EntityType Name="Department">
   <Key>
     <PropertyRef Name="DepartmentID" />
   </Key>
   <Property Type="Int32" Name="DepartmentID" Nullable="false" />
   <Property Type="String" Name="Name" Nullable="false"
             MaxLength="50" FixedLength="false" Unicode="true" />
   <Property Type="Decimal" Name="Budget" Nullable="false"
             Precision="19" Scale="4" />
   <Property Type="DateTime" Name="StartDate" Nullable="false" />
   <NavigationProperty Name="Courses"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Department" ToRole="Course" />
 </EntityType>
```

### <a name="example"></a><span data-ttu-id="59722-607">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-607">Example</span></span>

<span data-ttu-id="59722-608">Das nächste Beispiel zeigt das **QueryView** -Element als untergeordnetes Element eines **AssociationSetMapping** -Elements und definiert eine schreibgeschützte Zuordnung für die `FK_Course_Department`-Zuordnung im Modell "School".</span><span class="sxs-lookup"><span data-stu-id="59722-608">The next example shows the **QueryView** element as the child of an **AssociationSetMapping** element and defines a read-only mapping for the `FK_Course_Department` association in the School model.</span></span>

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolEntities">
   <EntitySetMapping Name="Courses" >
     <QueryView>
       SELECT VALUE SchoolModel.Course(c.CourseID,
                                       c.Title,
                                       c.Credits)
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments" >
     <QueryView>
       SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                           d.Name,
                                           d.Budget,
                                           d.StartDate)
       FROM SchoolModelStoreContainer.Department AS d
       WHERE d.Budget > 150000
     </QueryView>
   </EntitySetMapping>
   <AssociationSetMapping Name="FK_Course_Department" >
     <QueryView>
       SELECT VALUE SchoolModel.FK_Course_Department(
         CREATEREF(SchoolEntities.Departments, row(c.DepartmentID), SchoolModel.Department),
         CREATEREF(SchoolEntities.Courses, row(c.CourseID)) )
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </AssociationSetMapping>
 </EntityContainerMapping>
```
 
### <a name="comments"></a><span data-ttu-id="59722-609">Kommentare</span><span class="sxs-lookup"><span data-stu-id="59722-609">Comments</span></span>

<span data-ttu-id="59722-610">Sie können Abfragesichten definieren, um die folgenden Szenarien zu ermöglichen:</span><span class="sxs-lookup"><span data-stu-id="59722-610">You can define query views to enable the following scenarios:</span></span>

-   <span data-ttu-id="59722-611">Definieren einer Entität im konzeptionellen Modell, die nicht alle Eigenschaften der Entität im Speichermodell einschließt.</span><span class="sxs-lookup"><span data-stu-id="59722-611">Define an entity in the conceptual model that doesn't include all the properties of the entity in the storage model.</span></span> <span data-ttu-id="59722-612">Dies schließt Eigenschaften ein, die keine Standardwerte aufweisen und keine **null** -Werte unterstützen.</span><span class="sxs-lookup"><span data-stu-id="59722-612">This includes properties that do not have default values and do not support **null** values.</span></span>
-   <span data-ttu-id="59722-613">Zuordnen von berechneten Spalten im Speichermodell zu Eigenschaften von Entitätstypen im konzeptionellen Modell.</span><span class="sxs-lookup"><span data-stu-id="59722-613">Map computed columns in the storage model to properties of entity types in the conceptual model.</span></span>
-   <span data-ttu-id="59722-614">Definieren eines Mappings, bei dem die Bedingungen, die für die Partitionierung von Entitäten im konzeptionellen Modell verwendet werden, nicht auf Gleichheit basieren.</span><span class="sxs-lookup"><span data-stu-id="59722-614">Define a mapping where conditions used to partition entities in the conceptual model are not based on equality.</span></span> <span data-ttu-id="59722-615">Wenn Sie eine bedingte Zuordnung mit dem **Condition** -Element angeben, muss die angegebene Bedingung dem angegebenen Wert entsprechen.</span><span class="sxs-lookup"><span data-stu-id="59722-615">When you specify a conditional mapping using the **Condition** element, the supplied condition must equal the specified value.</span></span> <span data-ttu-id="59722-616">Weitere Informationen finden Sie unter Condition-Element (MSL).</span><span class="sxs-lookup"><span data-stu-id="59722-616">For more information, see Condition Element (MSL).</span></span>
-   <span data-ttu-id="59722-617">Zuordnen derselben Spalte im Speichermodell zu mehreren Typen im konzeptionellen Modell.</span><span class="sxs-lookup"><span data-stu-id="59722-617">Map the same column in the storage model to multiple types in the conceptual model.</span></span>
-   <span data-ttu-id="59722-618">Zuordnen mehrerer Typen zu derselben Tabelle.</span><span class="sxs-lookup"><span data-stu-id="59722-618">Map multiple types to the same table.</span></span>
-   <span data-ttu-id="59722-619">Definieren von Zuordnungen im konzeptionellen Modell, die nicht auf Fremdschlüsseln im relationalen Schema basieren.</span><span class="sxs-lookup"><span data-stu-id="59722-619">Define associations in the conceptual model that are not based on foreign keys in the relational schema.</span></span>
-   <span data-ttu-id="59722-620">Verwenden benutzerdefinierter Geschäftslogik, um die Werte von Eigenschaften im konzeptionellen Modell festzulegen.</span><span class="sxs-lookup"><span data-stu-id="59722-620">Use custom business logic to set the value of properties in the conceptual model.</span></span> <span data-ttu-id="59722-621">Beispielsweise können Sie den Zeichen folgen Wert "T" in der Datenquelle einem Wert von **true**, einem booleschen Wert im konzeptionellen Modell zuordnen.</span><span class="sxs-lookup"><span data-stu-id="59722-621">For example, you could map the string value "T" in the data source to a value of **true**, a Boolean, in the conceptual model.</span></span>
-   <span data-ttu-id="59722-622">Definieren bedingter Filter für Abfrageergebnisse.</span><span class="sxs-lookup"><span data-stu-id="59722-622">Define conditional filters for query results.</span></span>
-   <span data-ttu-id="59722-623">Erzwingen von geringeren Dateneinschränkungen im konzeptionellen Modell als im Speichermodell.</span><span class="sxs-lookup"><span data-stu-id="59722-623">Enforce fewer restrictions on data in the conceptual model than in the storage model.</span></span> <span data-ttu-id="59722-624">Beispielsweise können Sie festlegen, dass eine Eigenschaft im konzeptionellen Modell NULL-Werte zulässt, auch wenn die Spalte, der Sie zugeordnet ist, keine **null**-Werte unterstützt.</span><span class="sxs-lookup"><span data-stu-id="59722-624">For example, you could make a property in the conceptual model nullable even if the column to which it is mapped does not support **null**values.</span></span>

<span data-ttu-id="59722-625">Die folgenden Aspekte gelten, wenn Sie Abfragesichten für Entitäten definieren:</span><span class="sxs-lookup"><span data-stu-id="59722-625">The following considerations apply when you define query views for entities:</span></span>

-   <span data-ttu-id="59722-626">Abfragesichten sind schreibgeschützt.</span><span class="sxs-lookup"><span data-stu-id="59722-626">Query views are read-only.</span></span> <span data-ttu-id="59722-627">Sie können Entitäten nur mit Änderungsfunktionen aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="59722-627">You can only make updates to entities by using modification functions.</span></span>
-   <span data-ttu-id="59722-628">Wenn Sie eine Entität durch eine Abfragesicht definieren, müssen Sie auch alle verknüpften Entitäten durch Abfragesichten definieren.</span><span class="sxs-lookup"><span data-stu-id="59722-628">When you define an entity type by a query view, you must also define all related entities by query views.</span></span>
-   <span data-ttu-id="59722-629">Wenn Sie einer Entität im Speichermodell, das eine Verknüpfungs Tabelle im relationalen Schema darstellt, eine m:n-Zuordnung zuordnen, müssen Sie im **AssociationSetMapping** -Element für diese Link Tabelle ein **QueryView** -Element definieren.</span><span class="sxs-lookup"><span data-stu-id="59722-629">When you map a many-to-many association to an entity in the storage model that represents a link table in the relational schema, you must define a **QueryView** element in the **AssociationSetMapping** element for this link table.</span></span>
-   <span data-ttu-id="59722-630">Abfragesichten müssen für alle Typen in einer Typhierarchie definiert werden.</span><span class="sxs-lookup"><span data-stu-id="59722-630">Query views must be defined for all types in a type hierarchy.</span></span> <span data-ttu-id="59722-631">Dazu stehen Ihnen folgende Möglichkeiten zur Verfügung:</span><span class="sxs-lookup"><span data-stu-id="59722-631">You can do this in the following ways:</span></span>
-   -   <span data-ttu-id="59722-632">Mit einem einzelnen **QueryView** -Element, das eine einzelne Entity SQL Abfrage angibt, die eine Vereinigung aller Entitäts Typen in der Hierarchie zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="59722-632">With a single **QueryView** element that specifies a single Entity SQL query that returns a union of all of the entity types in the hierarchy.</span></span>
    -   <span data-ttu-id="59722-633">Mit einem einzelnen **QueryView** -Element, das eine einzelne Entity SQL Abfrage angibt, die den Case-Operator verwendet, um einen bestimmten Entitätstyp in der Hierarchie auf der Grundlage einer bestimmten Bedingung zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="59722-633">With a single **QueryView** element that specifies a single Entity SQL query that uses the CASE operator to return a specific entity type in the hierarchy based on a specific condition.</span></span>
    -   <span data-ttu-id="59722-634">Mit einem zusätzlichen **QueryView** -Element für einen bestimmten Typ in der Hierarchie.</span><span class="sxs-lookup"><span data-stu-id="59722-634">With an additional **QueryView** element for a specific type in the hierarchy.</span></span> <span data-ttu-id="59722-635">Verwenden Sie in diesem Fall das **tykame** -Attribut des **QueryView** -Elements, um den Entitätstyp für jede Ansicht anzugeben.</span><span class="sxs-lookup"><span data-stu-id="59722-635">In this case, use the **TypeName** attribute of the **QueryView** element to specify the entity type for each view.</span></span>
-   <span data-ttu-id="59722-636">Wenn eine Abfrage Sicht definiert ist, können Sie das **StorageSetName** -Attribut nicht im **EntitySetMapping** -Element angeben.</span><span class="sxs-lookup"><span data-stu-id="59722-636">When a query view is defined, you cannot specify the **StorageSetName** attribute on the **EntitySetMapping** element.</span></span>
-   <span data-ttu-id="59722-637">Wenn eine Abfrage Sicht definiert ist, kann das **EntitySetMapping**-Element nicht auch **Eigenschaften** Zuordnungen enthalten.</span><span class="sxs-lookup"><span data-stu-id="59722-637">When a query view is defined, the **EntitySetMapping**element cannot also contain **Property** mappings.</span></span>

## <a name="resultbinding-element-msl"></a><span data-ttu-id="59722-638">ResultBinding-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="59722-638">ResultBinding Element (MSL)</span></span>

<span data-ttu-id="59722-639">Das **ResultBinding** -Element in der Mapping-Spezifikationssprache (Mapping Specification Language, MSL) ordnet Spaltenwerte, die von gespeicherten Prozeduren zurückgegeben werden, den Entitäts Eigenschaften im konzeptionellen Modell zu.</span><span class="sxs-lookup"><span data-stu-id="59722-639">The **ResultBinding** element in mapping specification language (MSL) maps column values that are returned by stored procedures to entity properties in the conceptual model when entity type modification functions are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="59722-640">Wenn z. b. der Wert einer Identitäts Spalte von einer gespeicherten INSERT-Prozedur zurückgegeben wird, ordnet das **ResultBinding** -Element den zurückgegebenen Wert einer Entitätstyp Eigenschaft im konzeptionellen Modell zu.</span><span class="sxs-lookup"><span data-stu-id="59722-640">For example, when the value of an identity column is returned by an insert stored procedure, the **ResultBinding** element maps the returned value to an entity type property in the conceptual model.</span></span>

<span data-ttu-id="59722-641">Das **ResultBinding** -Element kann ein untergeordnetes Element des InsertFunction-Elements oder des UpdateFunction-Elements sein.</span><span class="sxs-lookup"><span data-stu-id="59722-641">The **ResultBinding** element can be child of the InsertFunction element or the UpdateFunction element.</span></span>

<span data-ttu-id="59722-642">Das **ResultBinding** -Element darf keine untergeordneten Elemente aufweisen.</span><span class="sxs-lookup"><span data-stu-id="59722-642">The **ResultBinding** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="59722-643">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="59722-643">Applicable Attributes</span></span>

<span data-ttu-id="59722-644">In der folgenden Tabelle werden die Attribute beschrieben, die für das **ResultBinding** -Element anwendbar sind:</span><span class="sxs-lookup"><span data-stu-id="59722-644">The following table describes the attributes that are applicable to the **ResultBinding** element:</span></span>

| <span data-ttu-id="59722-645">Attributname</span><span class="sxs-lookup"><span data-stu-id="59722-645">Attribute Name</span></span> | <span data-ttu-id="59722-646">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="59722-646">Is Required</span></span> | <span data-ttu-id="59722-647">value</span><span class="sxs-lookup"><span data-stu-id="59722-647">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="59722-648">**Name**</span><span class="sxs-lookup"><span data-stu-id="59722-648">**Name**</span></span>       | <span data-ttu-id="59722-649">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-649">Yes</span></span>         | <span data-ttu-id="59722-650">Der Name der Entitätseigenschaft im konzeptionellen Modell, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-650">The name of the entity property in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="59722-651">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="59722-651">**ColumnName**</span></span> | <span data-ttu-id="59722-652">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-652">Yes</span></span>         | <span data-ttu-id="59722-653">Der Name der Spalte, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-653">The name of the column being mapped.</span></span>                                          |

### <a name="example"></a><span data-ttu-id="59722-654">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-654">Example</span></span>

<span data-ttu-id="59722-655">Das folgende Beispiel basiert auf dem Modell "School" und zeigt ein **InsertFunction** -Element, das verwendet wird, um die Einfügefunktion des Entitäts Typs " **Person** " der gespeicherten Prozedur **InsertPerson** zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="59722-655">The following example is based on the School model and shows an **InsertFunction** element used to map the insert function of the **Person** entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="59722-656">(Die gespeicherte Prozedur **InsertPerson** wird im folgenden dargestellt und im Speichermodell deklariert.) Ein **ResultBinding** -Element wird verwendet, um einen Spaltenwert, der von der gespeicherten Prozedur (**NewPersonID**) zurückgegeben wird, einer Entitätstyp Eigenschaft (**PersonID**) zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="59722-656">(The **InsertPerson** stored procedure is shown below and is declared in the storage model.) A **ResultBinding** element is used to map a column value that is returned by the stored procedure (**NewPersonID**) to an entity type property (**PersonID**).</span></span>

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```

<span data-ttu-id="59722-657">Das folgende Transact-SQL beschreibt die gespeicherte Prozedur **InsertPerson** :</span><span class="sxs-lookup"><span data-stu-id="59722-657">The following Transact-SQL describes the **InsertPerson** stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[InsertPerson]
                                @LastName nvarchar(50),
                                @FirstName nvarchar(50),
                                @HireDate datetime,
                                @EnrollmentDate datetime
                                AS
                                INSERT INTO dbo.Person (LastName,
                                                                             FirstName,
                                                                             HireDate,
                                                                             EnrollmentDate)
                                VALUES (@LastName,
                                               @FirstName,
                                               @HireDate,
                                               @EnrollmentDate);
                                SELECT SCOPE_IDENTITY() as NewPersonID;
```

## <a name="resultmapping-element-msl"></a><span data-ttu-id="59722-658">ResultMapping-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="59722-658">ResultMapping Element (MSL)</span></span>

<span data-ttu-id="59722-659">Das **resultmapping** -Element in der Mapping-Spezifikationssprache (Mapping Specification Language, MSL) definiert die Zuordnung zwischen einem Funktions Import im konzeptionellen Modell und einer gespeicherten Prozedur in der zugrunde liegenden Datenbank, wenn Folgendes zutrifft:</span><span class="sxs-lookup"><span data-stu-id="59722-659">The **ResultMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="59722-660">Der Funktionsimport gibt einen Entitätstyp des konzeptionellen Modells oder einen komplexen Typ zurück.</span><span class="sxs-lookup"><span data-stu-id="59722-660">The function import returns a conceptual model entity type or complex type.</span></span>
-   <span data-ttu-id="59722-661">Die Namen der Spalten, die von der gespeicherten Prozedur zurückgegeben werden, entsprechen nicht exakt den Namen der Eigenschaften für den Entitätstyp oder den komplexem Typ.</span><span class="sxs-lookup"><span data-stu-id="59722-661">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the entity type or complex type.</span></span>

<span data-ttu-id="59722-662">Standardmäßig basiert die Zuordnung der von einer gespeicherten Prozedur zurückgegebenen Spalten zu einem Entitätstyp oder einem komplexen Typ auf den Spalten- und Eigenschaftennamen.</span><span class="sxs-lookup"><span data-stu-id="59722-662">By default, the mapping between the columns returned by a stored procedure and an entity type or complex type is based on column and property names.</span></span> <span data-ttu-id="59722-663">Wenn Spaltennamen nicht exakt mit Eigenschaftsnamen übereinstimmen, müssen Sie das **resultmapping** -Element verwenden, um die Zuordnung zu definieren.</span><span class="sxs-lookup"><span data-stu-id="59722-663">If column names do not exactly match property names, you must use the **ResultMapping** element to define the mapping.</span></span> <span data-ttu-id="59722-664">Ein Beispiel für die Standard Zuordnung finden Sie unter FunctionImportMapping-Element (MSL).</span><span class="sxs-lookup"><span data-stu-id="59722-664">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="59722-665">Das **resultmapping** -Element ist ein untergeordnetes Element des FunctionImportMapping-Elements.</span><span class="sxs-lookup"><span data-stu-id="59722-665">The **ResultMapping** element is a child element of the FunctionImportMapping element.</span></span>

<span data-ttu-id="59722-666">Das **resultmapping** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="59722-666">The **ResultMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="59722-667">EntityTypeMapping (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-667">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="59722-668">ComplexTypeMapping</span><span class="sxs-lookup"><span data-stu-id="59722-668">ComplexTypeMapping</span></span>

<span data-ttu-id="59722-669">Für das **resultmapping** -Element sind keine Attribute anwendbar.</span><span class="sxs-lookup"><span data-stu-id="59722-669">No attributes are applicable to the **ResultMapping** Element.</span></span>

### <a name="example"></a><span data-ttu-id="59722-670">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-670">Example</span></span>

<span data-ttu-id="59722-671">Sehen Sie sich die folgende gespeicherte Prozedur an:</span><span class="sxs-lookup"><span data-stu-id="59722-671">Consider the following stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

<span data-ttu-id="59722-672">Betrachten Sie auch den folgenden Entitätstyp des konzeptionellen Modells:</span><span class="sxs-lookup"><span data-stu-id="59722-672">Also consider the following conceptual model entity type:</span></span>

``` xml
 <EntityType Name="StudentGrade">
   <Key>
     <PropertyRef Name="EnrollmentID" />
   </Key>
   <Property Name="EnrollmentID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="CourseID" Type="Int32" Nullable="false" />
   <Property Name="StudentID" Type="Int32" Nullable="false" />
   <Property Name="Grade" Type="Decimal" Precision="3" Scale="2" />
 </EntityType>
```

<span data-ttu-id="59722-673">Um einen Funktions Import zu erstellen, der Instanzen des vorherigen Entitäts Typs zurückgibt, muss die Zuordnung zwischen den Spalten, die von der gespeicherten Prozedur zurückgegeben werden, und dem Entitätstyp in einem **resultmapping** -Element definiert werden:</span><span class="sxs-lookup"><span data-stu-id="59722-673">In order to create a function import that returns instances of the previous entity type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ResultMapping** element:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <EntityTypeMapping TypeName="SchoolModel.StudentGrade">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </EntityTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="scalarproperty-element-msl"></a><span data-ttu-id="59722-674">ScalarProperty-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="59722-674">ScalarProperty Element (MSL)</span></span>

<span data-ttu-id="59722-675">Das **ScalarProperty** -Element in der Mapping-Spezifikationssprache (Mapping Specification Language, MSL) ordnet eine Eigenschaft einem Entitätstyp des konzeptionellen Modells, einem komplexen Typ oder einer Zuordnung zu einer Tabellenspalte oder einem Parameter einer gespeicherten Prozedur in der zugrunde liegenden Datenbank zu</span><span class="sxs-lookup"><span data-stu-id="59722-675">The **ScalarProperty** element in mapping specification language (MSL) maps a property on a conceptual model entity type, complex type, or association to a table column or stored procedure parameter in the underlying database.</span></span>

> [!NOTE]
> <span data-ttu-id="59722-676">Gespeicherte Prozeduren, denen Änderungsfunktionen zugeordnet werden, müssen im Speichermodell deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="59722-676">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="59722-677">Weitere Informationen finden Sie unter Function-Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="59722-677">For more information, see Function Element (SSDL).</span></span>

<span data-ttu-id="59722-678">Das **ScalarProperty** -Element kann ein untergeordnetes Element der folgenden Elemente sein:</span><span class="sxs-lookup"><span data-stu-id="59722-678">The **ScalarProperty** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="59722-679">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="59722-679">MappingFragment</span></span>
-   <span data-ttu-id="59722-680">InsertFunction</span><span class="sxs-lookup"><span data-stu-id="59722-680">InsertFunction</span></span>
-   <span data-ttu-id="59722-681">UpdateFunction</span><span class="sxs-lookup"><span data-stu-id="59722-681">UpdateFunction</span></span>
-   <span data-ttu-id="59722-682">DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="59722-682">DeleteFunction</span></span>
-   <span data-ttu-id="59722-683">EndProperty</span><span class="sxs-lookup"><span data-stu-id="59722-683">EndProperty</span></span>
-   <span data-ttu-id="59722-684">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="59722-684">ComplexProperty</span></span>
-   <span data-ttu-id="59722-685">ResultMapping</span><span class="sxs-lookup"><span data-stu-id="59722-685">ResultMapping</span></span>

<span data-ttu-id="59722-686">Als untergeordnetes Element des Elements **MappingFragment**, **ComplexProperty**oder **EndProperty** ordnet das **ScalarProperty** -Element eine Eigenschaft im konzeptionellen Modell einer Spalte in der Datenbank zu.</span><span class="sxs-lookup"><span data-stu-id="59722-686">As a child of the **MappingFragment**, **ComplexProperty**, or **EndProperty** element, the **ScalarProperty** element maps a property in the conceptual model to a column in the database.</span></span> <span data-ttu-id="59722-687">Als untergeordnetes Element des Elements **InsertFunction**, **UpdateFunction**oder **DeleteFunction** ordnet das **ScalarProperty** -Element eine Eigenschaft im konzeptionellen Modell einem Parameter für gespeicherte Prozeduren zu.</span><span class="sxs-lookup"><span data-stu-id="59722-687">As a child of the **InsertFunction**, **UpdateFunction**, or **DeleteFunction** element, the **ScalarProperty** element maps a property in the conceptual model to a stored procedure parameter.</span></span>

<span data-ttu-id="59722-688">Das **ScalarProperty** -Element darf keine untergeordneten Elemente aufweisen.</span><span class="sxs-lookup"><span data-stu-id="59722-688">The **ScalarProperty** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="59722-689">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="59722-689">Applicable Attributes</span></span>

<span data-ttu-id="59722-690">Die Attribute, die auf das **ScalarProperty** -Element angewendet werden, unterscheiden sich abhängig von der Rolle des Elements.</span><span class="sxs-lookup"><span data-stu-id="59722-690">The attributes that apply to the **ScalarProperty** element differ depending on the role of the element.</span></span>

<span data-ttu-id="59722-691">In der folgenden Tabelle werden die Attribute beschrieben, die anwendbar sind, wenn das **ScalarProperty** -Element verwendet wird, um einer Spalte in der Datenbank eine Eigenschaft eines konzeptionellen Modells zuzuordnen:</span><span class="sxs-lookup"><span data-stu-id="59722-691">The following table describes the attributes that are applicable when the **ScalarProperty** element is used to map a conceptual model property to a column in the database:</span></span>

| <span data-ttu-id="59722-692">Attributname</span><span class="sxs-lookup"><span data-stu-id="59722-692">Attribute Name</span></span> | <span data-ttu-id="59722-693">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="59722-693">Is Required</span></span> | <span data-ttu-id="59722-694">value</span><span class="sxs-lookup"><span data-stu-id="59722-694">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="59722-695">**Name**</span><span class="sxs-lookup"><span data-stu-id="59722-695">**Name**</span></span>       | <span data-ttu-id="59722-696">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-696">Yes</span></span>         | <span data-ttu-id="59722-697">Der Name der Eigenschaft im konzeptionellen Modell, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-697">The name of the conceptual model property that is being mapped.</span></span> |
| <span data-ttu-id="59722-698">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="59722-698">**ColumnName**</span></span> | <span data-ttu-id="59722-699">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-699">Yes</span></span>         | <span data-ttu-id="59722-700">Der Name der Tabellenspalte, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-700">The name of the table column that is being mapped.</span></span>              |

<span data-ttu-id="59722-701">In der folgenden Tabelle werden die Attribute beschrieben, die für das **ScalarProperty** -Element gelten, wenn es verwendet wird, um eine Eigenschaft des konzeptionellen Modells einem Parameter für gespeicherte Prozeduren zuzuordnen:</span><span class="sxs-lookup"><span data-stu-id="59722-701">The following table describes the attributes that are applicable to the **ScalarProperty** element when it is used to map a conceptual model property to a stored procedure parameter:</span></span>

| <span data-ttu-id="59722-702">Attributname</span><span class="sxs-lookup"><span data-stu-id="59722-702">Attribute Name</span></span>    | <span data-ttu-id="59722-703">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="59722-703">Is Required</span></span> | <span data-ttu-id="59722-704">value</span><span class="sxs-lookup"><span data-stu-id="59722-704">Value</span></span>                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="59722-705">**Name**</span><span class="sxs-lookup"><span data-stu-id="59722-705">**Name**</span></span>          | <span data-ttu-id="59722-706">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-706">Yes</span></span>         | <span data-ttu-id="59722-707">Der Name der Eigenschaft im konzeptionellen Modell, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-707">The name of the conceptual model property that is being mapped.</span></span>                                                                                 |
| <span data-ttu-id="59722-708">**Parameter Name**</span><span class="sxs-lookup"><span data-stu-id="59722-708">**ParameterName**</span></span> | <span data-ttu-id="59722-709">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-709">Yes</span></span>         | <span data-ttu-id="59722-710">Der Name des Parameters, der zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-710">The name of the parameter that is being mapped.</span></span>                                                                                                 |
| <span data-ttu-id="59722-711">**Version**</span><span class="sxs-lookup"><span data-stu-id="59722-711">**Version**</span></span>       | <span data-ttu-id="59722-712">Nein</span><span class="sxs-lookup"><span data-stu-id="59722-712">No</span></span>          | <span data-ttu-id="59722-713">**Current** oder **Original** , abhängig davon, ob der aktuelle Wert oder der ursprüngliche Wert der-Eigenschaft für Parallelitäts Überprüfungen verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="59722-713">**Current** or **Original** depending on whether the current value or the original value of the property should be used for concurrency checks.</span></span> |

### <a name="example"></a><span data-ttu-id="59722-714">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-714">Example</span></span>

<span data-ttu-id="59722-715">Das folgende Beispiel zeigt das **ScalarProperty** -Element, das auf zweierlei Weise verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="59722-715">The following example shows the **ScalarProperty** element used in two ways:</span></span>

-   <span data-ttu-id="59722-716">, Um die Eigenschaften des Entitäts Typs **Person** den Spalten der **Person**-Tabelle zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="59722-716">To map the properties of the **Person** entity type to the columns of the **Person**table.</span></span>
-   <span data-ttu-id="59722-717">, Um die Eigenschaften des Entitäts Typs **Person** den Parametern der gespeicherten Prozedur **updateperson** zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="59722-717">To map the properties of the **Person** entity type to the parameters of the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="59722-718">Die gespeicherten Prozeduren werden im Speichermodell deklariert.</span><span class="sxs-lookup"><span data-stu-id="59722-718">The stored procedures are declared in the storage model.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="59722-719">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-719">Example</span></span>

<span data-ttu-id="59722-720">Das nächste Beispiel zeigt das **ScalarProperty** -Element, das verwendet wird, um die INSERT-und DELETE-Funktionen einer konzeptionellen Modell Zuordnung gespeicherten Prozeduren in der Datenbank zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="59722-720">The next example shows the **ScalarProperty** element used to map the insert and delete functions of a conceptual model association to stored procedures in the database.</span></span> <span data-ttu-id="59722-721">Die gespeicherten Prozeduren werden im Speichermodell deklariert.</span><span class="sxs-lookup"><span data-stu-id="59722-721">The stored procedures are declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="updatefunction-element-msl"></a><span data-ttu-id="59722-722">UpdateFunction-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="59722-722">UpdateFunction Element (MSL)</span></span>

<span data-ttu-id="59722-723">Das **UpdateFunction** -Element in der Mapping-Spezifikationssprache (Mapping Specification Language, MSL) ordnet die Aktualisierungs Funktion eines Entitäts Typs im konzeptionellen Modell einer gespeicherten Prozedur in der zugrunde liegenden Datenbank zu.</span><span class="sxs-lookup"><span data-stu-id="59722-723">The **UpdateFunction** element in mapping specification language (MSL) maps the update function of an entity type in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="59722-724">Gespeicherte Prozeduren, denen Änderungsfunktionen zugeordnet werden, müssen im Speichermodell deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="59722-724">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="59722-725">Weitere Informationen finden Sie unter Function-Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="59722-725">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
>  <span data-ttu-id="59722-726">Wenn Sie nicht alle drei Einfüge-, Aktualisierungs-oder Löschvorgänge eines Entitäts Typs gespeicherten Prozeduren zuordnen, schlagen die nicht zugeordneten Vorgänge fehl, wenn Sie zur Laufzeit ausgeführt werden und eine Update Exception ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="59722-726">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="59722-727">Das **UpdateFunction** -Element kann ein untergeordnetes Element des ModificationFunctionMapping-Elements sein und auf das EntityTypeMapping-Element angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="59722-727">The **UpdateFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element.</span></span>

<span data-ttu-id="59722-728">Das **UpdateFunction** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="59722-728">The **UpdateFunction** element can have the following child elements:</span></span>

-   <span data-ttu-id="59722-729">AssociationEnd (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-729">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="59722-730">ComplexProperty (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-730">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="59722-731">ResultBinding (null oder 1)</span><span class="sxs-lookup"><span data-stu-id="59722-731">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="59722-732">Scarlarproperty (0 (null) oder mehr)</span><span class="sxs-lookup"><span data-stu-id="59722-732">ScarlarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="59722-733">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="59722-733">Applicable Attributes</span></span>

<span data-ttu-id="59722-734">In der folgenden Tabelle werden die Attribute beschrieben, die auf das **UpdateFunction** -Element angewendet werden können.</span><span class="sxs-lookup"><span data-stu-id="59722-734">The following table describes the attributes that can be applied to the **UpdateFunction** element.</span></span>

| <span data-ttu-id="59722-735">Attributname</span><span class="sxs-lookup"><span data-stu-id="59722-735">Attribute Name</span></span>            | <span data-ttu-id="59722-736">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="59722-736">Is Required</span></span> | <span data-ttu-id="59722-737">value</span><span class="sxs-lookup"><span data-stu-id="59722-737">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="59722-738">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="59722-738">**FunctionName**</span></span>          | <span data-ttu-id="59722-739">Ja</span><span class="sxs-lookup"><span data-stu-id="59722-739">Yes</span></span>         | <span data-ttu-id="59722-740">Der mit einem Namespace qualifizierte Name der gespeicherten Prozedur, der die Aktualisierungsfunktion zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="59722-740">The namespace-qualified name of the stored procedure to which the update function is mapped.</span></span> <span data-ttu-id="59722-741">Die gespeicherte Prozedur muss im Speichermodell deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="59722-741">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="59722-742">**Rowsaffectedparameter**</span><span class="sxs-lookup"><span data-stu-id="59722-742">**RowsAffectedParameter**</span></span> | <span data-ttu-id="59722-743">Nein</span><span class="sxs-lookup"><span data-stu-id="59722-743">No</span></span>          | <span data-ttu-id="59722-744">Der Name des Ausgabeparameters, der die Anzahl der betroffenen Zeilen zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="59722-744">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

### <a name="example"></a><span data-ttu-id="59722-745">Beispiel</span><span class="sxs-lookup"><span data-stu-id="59722-745">Example</span></span>

<span data-ttu-id="59722-746">Das folgende Beispiel basiert auf dem Modell "School" und zeigt das **UpdateFunction** -Element, das verwendet wird, um die Aktualisierungs Funktion des Entitäts Typs " **Person** " der gespeicherten Prozedur " **updateperson** " zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="59722-746">The following example is based on the School model and shows the **UpdateFunction** element used to map update function of the **Person** entity type to the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="59722-747">Die gespeicherte Prozedur **updateperson** wird im Speichermodell deklariert.</span><span class="sxs-lookup"><span data-stu-id="59722-747">The **UpdatePerson** stored procedure is declared in the storage model.</span></span>

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
