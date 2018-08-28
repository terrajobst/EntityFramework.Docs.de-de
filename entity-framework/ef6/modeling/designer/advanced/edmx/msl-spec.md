---
title: MSL-Spezifikation - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 77dc7072c70b104188cd23974f32308960daebb6
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996030"
---
# <a name="msl-specification"></a><span data-ttu-id="964b7-102">MSL-Spezifikation</span><span class="sxs-lookup"><span data-stu-id="964b7-102">MSL Specification</span></span>
<span data-ttu-id="964b7-103">Mapping-Spezifikationssprache (MSL) wird eine XML-basierte Sprache, die die Zuordnung zwischen dem konzeptionellen Modell und Speichermodell einer Entity Framework-Anwendung beschreibt ist.</span><span class="sxs-lookup"><span data-stu-id="964b7-103">Mapping specification language (MSL) is an XML-based language that describes the mapping between the conceptual model and storage model of an Entity Framework application.</span></span>

<span data-ttu-id="964b7-104">In einer Entity Framework-Anwendung wird die Mapping-Metadaten aus einer MSL-Datei (geschrieben in MSL) zur Buildzeit geladen.</span><span class="sxs-lookup"><span data-stu-id="964b7-104">In an Entity Framework application, mapping metadata is loaded from an .msl file (written in MSL) at build time.</span></span> <span data-ttu-id="964b7-105">Entitätsframework verwendet Mapping-Metadaten zur Laufzeit verwendet, um Abfragen für das konzeptionelle Modell in datenspeicherspezifische Befehle zu übersetzen.</span><span class="sxs-lookup"><span data-stu-id="964b7-105">Entity Framework uses mapping metadata at runtime to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="964b7-106">Die Entity Framework Designer (EF-Designer) speichert Informationen über die Zuordnung in einer EDMX-Datei zur Entwurfszeit.</span><span class="sxs-lookup"><span data-stu-id="964b7-106">The Entity Framework Designer (EF Designer) stores mapping information in an .edmx file at design time.</span></span> <span data-ttu-id="964b7-107">Zur Erstellungszeit verwendet der Entity Designer Informationen in einer EDMX-Datei die MSL-Datei zu erstellen, die zur Laufzeit vom Entity Framework benötigt wird</span><span class="sxs-lookup"><span data-stu-id="964b7-107">At build time, the Entity Designer uses information in an .edmx file to create the .msl file that is needed by Entity Framework at runtime</span></span>

<span data-ttu-id="964b7-108">Namen aller konzeptionellen oder Speichermodelltypen, auf denen in MSL verwiesen wird, müssen mit dem jeweiligen Namespacenamen qualifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-108">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="964b7-109">Weitere Informationen zu den Namespacenamen des konzeptionellen Modells, finden Sie unter [CSDL-Spezifikation](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="964b7-109">For information about the conceptual model namespace name, see [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span> <span data-ttu-id="964b7-110">Weitere Informationen zu den Namespacenamen des Storage-Modell, finden Sie unter [SSDL-Spezifikation](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="964b7-110">For information about the storage model namespace name, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="964b7-111">MSL-Versionen unterscheiden sich von XML-Namespaces.</span><span class="sxs-lookup"><span data-stu-id="964b7-111">Versions of MSL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="964b7-112">MSL-Version</span><span class="sxs-lookup"><span data-stu-id="964b7-112">MSL Version</span></span> | <span data-ttu-id="964b7-113">XML-Namespace</span><span class="sxs-lookup"><span data-stu-id="964b7-113">XML Namespace</span></span>                                        |
|:------------|:-----------------------------------------------------|
| <span data-ttu-id="964b7-114">MSL v1</span><span class="sxs-lookup"><span data-stu-id="964b7-114">MSL v1</span></span>      | <span data-ttu-id="964b7-115">Urn: Schemas-Microsoft-Com:windows:storage:mapping:CS</span><span class="sxs-lookup"><span data-stu-id="964b7-115">urn:schemas-microsoft-com:windows:storage:mapping:CS</span></span> |
| <span data-ttu-id="964b7-116">MSL-v2</span><span class="sxs-lookup"><span data-stu-id="964b7-116">MSL v2</span></span>      | http://schemas.microsoft.com/ado/2008/09/mapping/cs  |
| <span data-ttu-id="964b7-117">MSL v3</span><span class="sxs-lookup"><span data-stu-id="964b7-117">MSL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a><span data-ttu-id="964b7-118">Alias-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="964b7-118">Alias Element (MSL)</span></span>

<span data-ttu-id="964b7-119">Die **Alias** -Element der mapping-Spezifikationssprache (MSL) ist ein untergeordnetes Element des das Mapping-Element, das verwendet wird, um Aliase für konzeptionelle Modell und speichermodellnamespaces zu definieren.</span><span class="sxs-lookup"><span data-stu-id="964b7-119">The **Alias** element in mapping specification language (MSL) is a child of the Mapping element that is used to define aliases for conceptual model and storage model namespaces.</span></span> <span data-ttu-id="964b7-120">Namen aller konzeptionellen oder Speichermodelltypen, auf denen in MSL verwiesen wird, müssen mit dem jeweiligen Namespacenamen qualifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-120">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="964b7-121">Informationen zu den Namespacenamen des konzeptionellen Modells finden Sie in der Schema-Element (CSDL).</span><span class="sxs-lookup"><span data-stu-id="964b7-121">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="964b7-122">Informationen zu den Namespacenamen des Storage-Modell finden Sie in der Schema-Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="964b7-122">For information about the storage model namespace name, see Schema Element (SSDL).</span></span>

<span data-ttu-id="964b7-123">Die **Alias** Element darf keine untergeordneten Elemente.</span><span class="sxs-lookup"><span data-stu-id="964b7-123">The **Alias** element cannot have child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="964b7-124">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="964b7-124">Applicable Attributes</span></span>

<span data-ttu-id="964b7-125">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Alias** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-125">The table below describes the attributes that can be applied to the **Alias** element.</span></span>

| <span data-ttu-id="964b7-126">Attributname</span><span class="sxs-lookup"><span data-stu-id="964b7-126">Attribute Name</span></span> | <span data-ttu-id="964b7-127">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="964b7-127">Is Required</span></span> | <span data-ttu-id="964b7-128">Wert</span><span class="sxs-lookup"><span data-stu-id="964b7-128">Value</span></span>                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| <span data-ttu-id="964b7-129">**Key**</span><span class="sxs-lookup"><span data-stu-id="964b7-129">**Key**</span></span>        | <span data-ttu-id="964b7-130">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-130">Yes</span></span>         | <span data-ttu-id="964b7-131">Der Alias für den Namespace, der angegeben wird die **Wert** Attribut.</span><span class="sxs-lookup"><span data-stu-id="964b7-131">The alias for the namespace that is specified by the **Value** attribute.</span></span> |
| <span data-ttu-id="964b7-132">**Wert**</span><span class="sxs-lookup"><span data-stu-id="964b7-132">**Value**</span></span>      | <span data-ttu-id="964b7-133">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-133">Yes</span></span>         | <span data-ttu-id="964b7-134">Der Namespace für die der Wert des der **Schlüssel** Element ist ein Alias.</span><span class="sxs-lookup"><span data-stu-id="964b7-134">The namespace for which the value of the **Key** element is an alias.</span></span>     |

### <a name="example"></a><span data-ttu-id="964b7-135">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-135">Example</span></span>

<span data-ttu-id="964b7-136">Das folgende Beispiel zeigt eine **Alias** Element, das einen Alias definiert `c`, für die Typen, die im konzeptionellen Modell definiert sind.</span><span class="sxs-lookup"><span data-stu-id="964b7-136">The following example shows an **Alias** element that defines an alias, `c`, for types that are defined in the conceptual model.</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="http://schemas.microsoft.com/ado/2009/11/mapping/cs">
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

## <a name="associationend-element-msl"></a><span data-ttu-id="964b7-137">AssociationEnd-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="964b7-137">AssociationEnd Element (MSL)</span></span>

<span data-ttu-id="964b7-138">Die **AssociationEnd** -Element der mapping-Spezifikationssprache (MSL) wird verwendet, wenn die Änderungsfunktionen eines Entitätstyps im konzeptionellen Modell gespeicherten Prozeduren in der zugrunde liegenden Datenbank zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-138">The **AssociationEnd** element in mapping specification language (MSL) is used when the modification functions of an entity type in the conceptual model are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="964b7-139">Wenn eine Änderung, die gespeicherte Prozedur einen Parameter übernimmt, dessen Wert wird in einer Zuordnungseigenschaft aufrechterhalten, der **AssociationEnd** Element wird der Parameter den Wert der Eigenschaft zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="964b7-139">If a modification stored procedure takes a parameter whose value is held in an association property, the **AssociationEnd** element maps the property value to the parameter.</span></span> <span data-ttu-id="964b7-140">Weitere Informationen finden Sie im untenstehenden Beispiel.</span><span class="sxs-lookup"><span data-stu-id="964b7-140">For more information, see the example below.</span></span>

<span data-ttu-id="964b7-141">Weitere Informationen zum Zuordnen von Änderungsfunktionen von Entitätstypen zu gespeicherten Prozeduren finden Sie unter ModificationFunctionMapping-Element (MSL) und die exemplarische Vorgehensweise: Zuordnen einer Entität zu gespeicherten Prozeduren.</span><span class="sxs-lookup"><span data-stu-id="964b7-141">For more information about mapping modification functions of entity types to stored procedures, see ModificationFunctionMapping Element (MSL) and Walkthrough: Mapping an Entity to Stored Procedures.</span></span>

<span data-ttu-id="964b7-142">Die **AssociationEnd** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="964b7-142">The **AssociationEnd** element can have the following child elements:</span></span>

-   <span data-ttu-id="964b7-143">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="964b7-143">ScalarProperty</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="964b7-144">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="964b7-144">Applicable Attributes</span></span>

<span data-ttu-id="964b7-145">Die folgende Tabelle beschreibt die Attribute für die **AssociationEnd** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-145">The following table describes the attributes that are applicable to the **AssociationEnd** element.</span></span>

| <span data-ttu-id="964b7-146">Attributname</span><span class="sxs-lookup"><span data-stu-id="964b7-146">Attribute Name</span></span>     | <span data-ttu-id="964b7-147">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="964b7-147">Is Required</span></span> | <span data-ttu-id="964b7-148">Wert</span><span class="sxs-lookup"><span data-stu-id="964b7-148">Value</span></span>                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="964b7-149">**AssociationSet**</span><span class="sxs-lookup"><span data-stu-id="964b7-149">**AssociationSet**</span></span> | <span data-ttu-id="964b7-150">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-150">Yes</span></span>         | <span data-ttu-id="964b7-151">Der Name der Zuordnung, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-151">The name of the association that is being mapped.</span></span>                                                                                                                                 |
| <span data-ttu-id="964b7-152">**From**</span><span class="sxs-lookup"><span data-stu-id="964b7-152">**From**</span></span>           | <span data-ttu-id="964b7-153">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-153">Yes</span></span>         | <span data-ttu-id="964b7-154">Der Wert des der **FromRole** Attribut der Navigationseigenschaft, die der zugeordneten Zuordnung entspricht.</span><span class="sxs-lookup"><span data-stu-id="964b7-154">The value of the **FromRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="964b7-155">Weitere Informationen finden Sie unter NavigationProperty-Element (CSDL).</span><span class="sxs-lookup"><span data-stu-id="964b7-155">For more information, see NavigationProperty Element (CSDL).</span></span> |
| <span data-ttu-id="964b7-156">**Aktion**</span><span class="sxs-lookup"><span data-stu-id="964b7-156">**To**</span></span>             | <span data-ttu-id="964b7-157">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-157">Yes</span></span>         | <span data-ttu-id="964b7-158">Der Wert des der **ToRole** Attribut der Navigationseigenschaft, die der zugeordneten Zuordnung entspricht.</span><span class="sxs-lookup"><span data-stu-id="964b7-158">The value of the **ToRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="964b7-159">Weitere Informationen finden Sie unter NavigationProperty-Element (CSDL).</span><span class="sxs-lookup"><span data-stu-id="964b7-159">For more information, see NavigationProperty Element (CSDL).</span></span>   |

### <a name="example"></a><span data-ttu-id="964b7-160">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-160">Example</span></span>

<span data-ttu-id="964b7-161">Betrachten Sie den folgenden Entitätstyp des konzeptionellen Modells:</span><span class="sxs-lookup"><span data-stu-id="964b7-161">Consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="964b7-162">Betrachten Sie auch die folgende gespeicherte Prozedur:</span><span class="sxs-lookup"><span data-stu-id="964b7-162">Also consider the following stored procedure:</span></span>

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

<span data-ttu-id="964b7-163">Um die Aktualisierungsfunktion der `Course` Entität dieser gespeicherten Prozedur müssen Sie einen Wert angeben der **"DepartmentID"** Parameter.</span><span class="sxs-lookup"><span data-stu-id="964b7-163">In order to map the update function of the `Course` entity to this stored procedure, you must supply a value to the **DepartmentID** parameter.</span></span> <span data-ttu-id="964b7-164">Der Wert für `DepartmentID` entspricht keiner Eigenschaft des Entitätstyps; er ist vielmehr in einer unabhängigen Zuordnung enthalten, deren Zuordnung hier angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="964b7-164">The value for `DepartmentID` does not correspond to a property on the entity type; it is contained in an independent association whose mapping is shown here:</span></span>

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

<span data-ttu-id="964b7-165">Der folgende code zeigt die **AssociationEnd** Element für die Zuordnung der **"DepartmentID"** Eigenschaft der **FK\_Kurs\_Abteilung** Zuordnung, die die **UpdateCourse** gespeicherten Prozedur (zu dem die Aktualisierungsfunktion des der **Kurs** Entitätstyp zugeordnet ist):</span><span class="sxs-lookup"><span data-stu-id="964b7-165">The following code shows the **AssociationEnd** element used to map the **DepartmentID** property of the **FK\_Course\_Department** association to the **UpdateCourse** stored procedure (to which the update function of the **Course** entity type is mapped):</span></span>

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

## <a name="associationsetmapping-element-msl"></a><span data-ttu-id="964b7-166">AssociationSetMapping-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="964b7-166">AssociationSetMapping Element (MSL)</span></span>

<span data-ttu-id="964b7-167">Die **AssociationSetMapping** -Element der mapping-Spezifikationssprache (MSL) definiert die Zuordnung zwischen einer Zuordnung im konzeptionellen Modell und zur Tabelle Spalten der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="964b7-167">The **AssociationSetMapping** element in mapping specification language (MSL) defines the mapping between an association in the conceptual model and table columns in the underlying database.</span></span>

<span data-ttu-id="964b7-168">Zuordnungen im konzeptionellen Modell sind Typen, deren Eigenschaften Primär- und Fremdschlüsselspalten in der zugrunde liegenden Datenbank darstellen.</span><span class="sxs-lookup"><span data-stu-id="964b7-168">Associations in the conceptual model are types whose properties represent primary and foreign key columns in the underlying database.</span></span> <span data-ttu-id="964b7-169">Die **AssociationSetMapping** -Element verwendet zwei EndProperty-Elemente, um die Zuordnungen zwischen zuordnungstypeigenschaften zu Spalten in der Datenbank zu definieren.</span><span class="sxs-lookup"><span data-stu-id="964b7-169">The **AssociationSetMapping** element uses two EndProperty elements to define the mappings between association type properties and columns in the database.</span></span> <span data-ttu-id="964b7-170">Sie können Bedingungen für diese Zuordnungen mit der Condition-Element platzieren.</span><span class="sxs-lookup"><span data-stu-id="964b7-170">You can place conditions on these mappings with the Condition element.</span></span> <span data-ttu-id="964b7-171">Ordnen Sie die INSERT-, Update- und Löschfunktionen für Zuordnungen zu gespeicherten Prozeduren in der Datenbank mit dem ModificationFunctionMapping-Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-171">Map the insert, update, and delete functions for associations to stored procedures in the database with the ModificationFunctionMapping element.</span></span> <span data-ttu-id="964b7-172">Definieren Sie schreibgeschützte Zuordnungen von Zuordnungen zu Spalten der Tabelle, indem Sie mit einer Entity SQL-Zeichenfolge in ein QueryView-Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-172">Define read-only mappings between associations and table columns by using an Entity SQL string in a QueryView element.</span></span>

> [!NOTE]
> <span data-ttu-id="964b7-173">Wenn eine referenzielle Einschränkung für eine Zuordnung im konzeptionellen Modell definiert ist, die Zuordnung muss nicht zugeordnet ist ein **AssociationSetMapping** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-173">If a referential constraint is defined for an association in the conceptual model, the association does not need to be mapped with an **AssociationSetMapping** element.</span></span> <span data-ttu-id="964b7-174">Wenn ein **AssociationSetMapping** -Element für eine Zuordnung, die eine referenzielle Einschränkung vorhanden ist, wird die Zuordnungen definiert, der **AssociationSetMapping** Element wird ignoriert.</span><span class="sxs-lookup"><span data-stu-id="964b7-174">If an **AssociationSetMapping** element is present for an association that has a referential constraint, the mappings defined in the **AssociationSetMapping** element will be ignored.</span></span> <span data-ttu-id="964b7-175">Weitere Informationen finden Sie unter ReferentialConstraint-Element (CSDL).</span><span class="sxs-lookup"><span data-stu-id="964b7-175">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="964b7-176">Die **AssociationSetMapping** -Element kann die folgenden untergeordneten Elemente aufweisen</span><span class="sxs-lookup"><span data-stu-id="964b7-176">The **AssociationSetMapping** element can have the following child elements</span></span>

-   <span data-ttu-id="964b7-177">QueryView (0 (null) oder 1)</span><span class="sxs-lookup"><span data-stu-id="964b7-177">QueryView (zero or one)</span></span>
-   <span data-ttu-id="964b7-178">EndProperty (0 (null) oder zwei)</span><span class="sxs-lookup"><span data-stu-id="964b7-178">EndProperty (zero or two)</span></span>
-   <span data-ttu-id="964b7-179">Bedingung (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-179">Condition (zero or more)</span></span>
-   <span data-ttu-id="964b7-180">ModificationFunctionMapping (0 (null) oder 1)</span><span class="sxs-lookup"><span data-stu-id="964b7-180">ModificationFunctionMapping (zero or one)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="964b7-181">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="964b7-181">Applicable Attributes</span></span>

<span data-ttu-id="964b7-182">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **AssociationSetMapping** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-182">The following table describes the attributes that can be applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="964b7-183">Attributname</span><span class="sxs-lookup"><span data-stu-id="964b7-183">Attribute Name</span></span>     | <span data-ttu-id="964b7-184">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="964b7-184">Is Required</span></span> | <span data-ttu-id="964b7-185">Wert</span><span class="sxs-lookup"><span data-stu-id="964b7-185">Value</span></span>                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| <span data-ttu-id="964b7-186">**Name**</span><span class="sxs-lookup"><span data-stu-id="964b7-186">**Name**</span></span>           | <span data-ttu-id="964b7-187">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-187">Yes</span></span>         | <span data-ttu-id="964b7-188">Der Name des konzeptionellen Modell-Zuordnungssatzes, der zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-188">The name of the conceptual model association set that is being mapped.</span></span>                      |
| <span data-ttu-id="964b7-189">**Typname**</span><span class="sxs-lookup"><span data-stu-id="964b7-189">**TypeName**</span></span>       | <span data-ttu-id="964b7-190">Nein</span><span class="sxs-lookup"><span data-stu-id="964b7-190">No</span></span>          | <span data-ttu-id="964b7-191">Der mit einem Namespace qualifizierte Name des konzeptionellen Modell-Zuordnungstyps, der zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-191">The namespace-qualified name of the conceptual model association type that is being mapped.</span></span> |
| <span data-ttu-id="964b7-192">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="964b7-192">**StoreEntitySet**</span></span> | <span data-ttu-id="964b7-193">Nein</span><span class="sxs-lookup"><span data-stu-id="964b7-193">No</span></span>          | <span data-ttu-id="964b7-194">Der Name der Tabelle, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-194">The name of the table that is being mapped.</span></span>                                                 |

### <a name="example"></a><span data-ttu-id="964b7-195">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-195">Example</span></span>

<span data-ttu-id="964b7-196">Das folgende Beispiel zeigt eine **AssociationSetMapping** Element, in dem die **FK\_Kurs\_Abteilung** der ZuordnungssatzimkonzeptionellenModellzugeordnetist **Kurs** Tabelle in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="964b7-196">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association set in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="964b7-197">Zuordnungen zwischen zuordnungstypeigenschaften zu Tabellenspalten werden in untergeordneten angegeben **EndProperty** Elemente.</span><span class="sxs-lookup"><span data-stu-id="964b7-197">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

## <a name="complexproperty-element-msl"></a><span data-ttu-id="964b7-198">ComplexProperty-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="964b7-198">ComplexProperty Element (MSL)</span></span>

<span data-ttu-id="964b7-199">Ein **ComplexProperty** -Element der mapping-Spezifikationssprache (MSL) definiert die Zuordnung zwischen der Eigenschaft eines komplexen Typs in einem konzeptionellen Modell Entität den Tabellenspalten in der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="964b7-199">A **ComplexProperty** element in mapping specification language (MSL) defines the mapping between a complex type property on a conceptual model entity type and table columns in the underlying database.</span></span> <span data-ttu-id="964b7-200">Die Zuordnungen zu Eigenschaftenspalten sind in untergeordneten ScalarProperty-Elemente angegeben.</span><span class="sxs-lookup"><span data-stu-id="964b7-200">The property-column mappings are specified in child ScalarProperty elements.</span></span>

<span data-ttu-id="964b7-201">Die **ComplexType** Property-Element kann die folgenden untergeordneten Elemente verfügen:</span><span class="sxs-lookup"><span data-stu-id="964b7-201">The **ComplexType** property element can have the following child elements:</span></span>

-   <span data-ttu-id="964b7-202">ScalarProperty (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-202">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="964b7-203">**ComplexProperty** (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-203">**ComplexProperty** (zero or more)</span></span>
-   <span data-ttu-id="964b7-204">ComplextTypeMapping (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-204">ComplextTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="964b7-205">Bedingung (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-205">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="964b7-206">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="964b7-206">Applicable Attributes</span></span>

<span data-ttu-id="964b7-207">Die folgende Tabelle beschreibt die Attribute für die **ComplexProperty** Element:</span><span class="sxs-lookup"><span data-stu-id="964b7-207">The following table describes the attributes that are applicable to the **ComplexProperty** element:</span></span>

| <span data-ttu-id="964b7-208">Attributname</span><span class="sxs-lookup"><span data-stu-id="964b7-208">Attribute Name</span></span> | <span data-ttu-id="964b7-209">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="964b7-209">Is Required</span></span> | <span data-ttu-id="964b7-210">Wert</span><span class="sxs-lookup"><span data-stu-id="964b7-210">Value</span></span>                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="964b7-211">**Name**</span><span class="sxs-lookup"><span data-stu-id="964b7-211">**Name**</span></span>       | <span data-ttu-id="964b7-212">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-212">Yes</span></span>         | <span data-ttu-id="964b7-213">Der Name der komplexen Eigenschaft eines Entitätstyps im konzeptionellen Modell, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-213">The name of the complex property of an entity type in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="964b7-214">**Typname**</span><span class="sxs-lookup"><span data-stu-id="964b7-214">**TypeName**</span></span>   | <span data-ttu-id="964b7-215">Nein</span><span class="sxs-lookup"><span data-stu-id="964b7-215">No</span></span>          | <span data-ttu-id="964b7-216">Der mit einem Namespace qualifizierte Name des Eigenschaftentyps im konzeptionellen Modell.</span><span class="sxs-lookup"><span data-stu-id="964b7-216">The namespace-qualified name of the conceptual model property type.</span></span>                              |

### <a name="example"></a><span data-ttu-id="964b7-217">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-217">Example</span></span>

<span data-ttu-id="964b7-218">Das folgende Beispiel basiert auf dem Modell "School".</span><span class="sxs-lookup"><span data-stu-id="964b7-218">The following example is based on the School model.</span></span> <span data-ttu-id="964b7-219">Dem konzeptionellen Modell wurde der folgende komplexe Typ hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="964b7-219">The following complex type has been added to the conceptual model:</span></span>

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

<span data-ttu-id="964b7-220">Die **"LastName"** und **FirstName** Eigenschaften der **Person** Entitätstyp wurden durch eine komplexe Eigenschaft ersetzt **Namen**:</span><span class="sxs-lookup"><span data-stu-id="964b7-220">The **LastName** and **FirstName** properties of the **Person** entity type have been replaced with one complex property, **Name**:</span></span>

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

<span data-ttu-id="964b7-221">Die folgende MSL-zeigt die **ComplexProperty** Element für die Zuordnung der **Namen** -Eigenschaft den Spalten in der zugrunde liegenden Datenbank:</span><span class="sxs-lookup"><span data-stu-id="964b7-221">The following MSL shows the **ComplexProperty** element used to map the **Name** property to columns in the underlying database:</span></span>

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

## <a name="complextypemapping-element-msl"></a><span data-ttu-id="964b7-222">ComplexTypeMapping-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="964b7-222">ComplexTypeMapping Element (MSL)</span></span>

<span data-ttu-id="964b7-223">Die **ComplexTypeMapping** -Element der mapping-Spezifikationssprache (MSL) ist ein untergeordnetes Element von der ResultMapping-Element und definiert die Zuordnung zwischen einem Funktionsimport im konzeptionellen Modell und eine gespeicherte Prozedur in der zugrunde liegenden Datenbank, wenn Folgendes zutrifft:</span><span class="sxs-lookup"><span data-stu-id="964b7-223">The **ComplexTypeMapping** element in mapping specification language (MSL) is a child of the ResultMapping element and defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="964b7-224">Der Funktionsimport gibt einen konzeptionellen komplexen Typ zurück.</span><span class="sxs-lookup"><span data-stu-id="964b7-224">The function import returns a conceptual complex type.</span></span>
-   <span data-ttu-id="964b7-225">Die Namen der Spalten, die von der gespeicherten Prozedur zurückgegeben werden, entsprechen nicht genau den Namen der Eigenschaften für den komplexen Typ.</span><span class="sxs-lookup"><span data-stu-id="964b7-225">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the complex type.</span></span>

<span data-ttu-id="964b7-226">Standardmäßig basiert die Zuordnung der von einer gespeicherten Prozedur zurückgegebenen Spalten zu einem komplexen Typ auf den Spalten- und Eigenschaftennamen.</span><span class="sxs-lookup"><span data-stu-id="964b7-226">By default, the mapping between the columns returned by a stored procedure and a complex type is based on column and property names.</span></span> <span data-ttu-id="964b7-227">Wenn Spaltennamen nicht exakt Eigenschaftennamen übereinstimmen, müssen Sie verwenden die **ComplexTypeMapping** Element, um die Zuordnung zu definieren.</span><span class="sxs-lookup"><span data-stu-id="964b7-227">If column names do not exactly match property names, you must use the **ComplexTypeMapping** element to define the mapping.</span></span> <span data-ttu-id="964b7-228">Ein Beispiel für die standardzuordnung finden Sie unter FunctionImportMapping-Element (MSL).</span><span class="sxs-lookup"><span data-stu-id="964b7-228">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="964b7-229">Die **ComplexTypeMapping** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="964b7-229">The **ComplexTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="964b7-230">ScalarProperty (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-230">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="964b7-231">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="964b7-231">Applicable Attributes</span></span>

<span data-ttu-id="964b7-232">Die folgende Tabelle beschreibt die Attribute für die **ComplexTypeMapping** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-232">The following table describes the attributes that are applicable to the **ComplexTypeMapping** element.</span></span>

| <span data-ttu-id="964b7-233">Attributname</span><span class="sxs-lookup"><span data-stu-id="964b7-233">Attribute Name</span></span> | <span data-ttu-id="964b7-234">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="964b7-234">Is Required</span></span> | <span data-ttu-id="964b7-235">Wert</span><span class="sxs-lookup"><span data-stu-id="964b7-235">Value</span></span>                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| <span data-ttu-id="964b7-236">**Typname**</span><span class="sxs-lookup"><span data-stu-id="964b7-236">**TypeName**</span></span>   | <span data-ttu-id="964b7-237">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-237">Yes</span></span>         | <span data-ttu-id="964b7-238">Der namespacequalifizierte Name des komplexen Typs, der zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-238">The namespace-qualified name of the complex type that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="964b7-239">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-239">Example</span></span>

<span data-ttu-id="964b7-240">Betrachten Sie die folgende gespeicherte Prozedur:</span><span class="sxs-lookup"><span data-stu-id="964b7-240">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="964b7-241">Betrachten Sie auch den folgenden komplexen Typ des konzeptionellen Modells:</span><span class="sxs-lookup"><span data-stu-id="964b7-241">Also consider the following conceptual model complex type:</span></span>

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

<span data-ttu-id="964b7-242">Um einen Funktionsimport zu erstellen, die Instanzen des vorherigen komplexen Typs zurückgibt, die Zuordnung zwischen den Spalten, die von der gespeicherten Prozedur zurückgegeben, und der Entitätstyp muss definiert werden, einem **ComplexTypeMapping** Element:</span><span class="sxs-lookup"><span data-stu-id="964b7-242">In order to create a function import that returns instances of the previous complex type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ComplexTypeMapping** element:</span></span>

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

## <a name="condition-element-msl"></a><span data-ttu-id="964b7-243">Condition-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="964b7-243">Condition Element (MSL)</span></span>

<span data-ttu-id="964b7-244">Die **Bedingung** -Element der mapping-Spezifikationssprache (MSL) definiert Bedingungen für Zuordnungen zwischen dem konzeptionellen Modell und der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="964b7-244">The **Condition** element in mapping specification language (MSL) places conditions on mappings between the conceptual model and the underlying database.</span></span> <span data-ttu-id="964b7-245">Die, die innerhalb eines XML-Knotens definierte Zuordnung ist gültig, wenn alle Bedingungen im angegebenen untergeordneten **Bedingung** Elemente, erfüllt werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-245">The mapping that is defined within an XML node is valid if all conditions, as specified in child **Condition** elements, are met.</span></span> <span data-ttu-id="964b7-246">Andernfalls ist die Zuordnung ungültig.</span><span class="sxs-lookup"><span data-stu-id="964b7-246">Otherwise, the mapping is not valid.</span></span> <span data-ttu-id="964b7-247">Angenommen, ein MappingFragment-Element enthält, eine oder mehrere **Bedingung** untergeordnete Elemente, die Zuordnung definiert, in der **MappingFragment** Knoten werden nur gültig, wenn alle der Bedingungen für die untergeordneten  **Bedingung** Elemente erfüllt werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-247">For example, if a MappingFragment element contains one or more **Condition** child elements, the mapping defined within the **MappingFragment** node will only be valid if all the conditions of the child **Condition** elements are met.</span></span>

<span data-ttu-id="964b7-248">Jede Bedingung kann zutreffen, entweder eine **Namen** (der Name der Entitätseigenschaft einer konzeptionellen Modell anhand des der **Namen** Attribut), oder ein **ColumnName** (der Name einer Spalte in die Datenbank, die gemäß der **ColumnName** Attribut).</span><span class="sxs-lookup"><span data-stu-id="964b7-248">Each condition can apply to either a **Name** (the name of a conceptual model entity property, specified by the **Name** attribute), or a **ColumnName** (the name of a column in the database, specified by the **ColumnName** attribute).</span></span> <span data-ttu-id="964b7-249">Wenn die **Namen** Attribut festgelegt ist, wird die Bedingung für einen Eigenschaftswert für die Entität aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="964b7-249">When the **Name** attribute is set, the condition is checked against an entity property value.</span></span> <span data-ttu-id="964b7-250">Wenn die **ColumnName** Attribut festgelegt ist, wird die Bedingung mit einem Spaltenwert aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="964b7-250">When the **ColumnName** attribute is set, the condition is checked against a column value.</span></span> <span data-ttu-id="964b7-251">Nur eine von der **Namen** oder **ColumnName** -Attribut angegeben werden, eine **Bedingung** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-251">Only one of the **Name** or **ColumnName** attribute can be specified in a **Condition** element.</span></span>

> [!NOTE]
> <span data-ttu-id="964b7-252">Wenn die **Bedingung** Element wird verwendet, in ein FunctionImportMapping-Element nur die **Namen** Attribut ist nicht anwendbar.</span><span class="sxs-lookup"><span data-stu-id="964b7-252">When the **Condition** element is used within a FunctionImportMapping element, only the **Name** attribute is not applicable.</span></span>

<span data-ttu-id="964b7-253">Die **Bedingung** Element kann ein untergeordnetes Element der folgenden Elemente sein:</span><span class="sxs-lookup"><span data-stu-id="964b7-253">The **Condition** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="964b7-254">AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="964b7-254">AssociationSetMapping</span></span>
-   <span data-ttu-id="964b7-255">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="964b7-255">ComplexProperty</span></span>
-   <span data-ttu-id="964b7-256">EntitySetMapping</span><span class="sxs-lookup"><span data-stu-id="964b7-256">EntitySetMapping</span></span>
-   <span data-ttu-id="964b7-257">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="964b7-257">MappingFragment</span></span>
-   <span data-ttu-id="964b7-258">EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="964b7-258">EntityTypeMapping</span></span>

<span data-ttu-id="964b7-259">Die **Bedingung** -Element kann keine untergeordneten Elemente aufweisen.</span><span class="sxs-lookup"><span data-stu-id="964b7-259">The **Condition** element can have no child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="964b7-260">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="964b7-260">Applicable Attributes</span></span>

<span data-ttu-id="964b7-261">Die folgende Tabelle beschreibt die Attribute für die **Bedingung** Element:</span><span class="sxs-lookup"><span data-stu-id="964b7-261">The following table describes the attributes that are applicable to the **Condition** element:</span></span>

| <span data-ttu-id="964b7-262">Attributname</span><span class="sxs-lookup"><span data-stu-id="964b7-262">Attribute Name</span></span> | <span data-ttu-id="964b7-263">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="964b7-263">Is Required</span></span> | <span data-ttu-id="964b7-264">Wert</span><span class="sxs-lookup"><span data-stu-id="964b7-264">Value</span></span>                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="964b7-265">**Spaltenname**</span><span class="sxs-lookup"><span data-stu-id="964b7-265">**ColumnName**</span></span> | <span data-ttu-id="964b7-266">Nein</span><span class="sxs-lookup"><span data-stu-id="964b7-266">No</span></span>          | <span data-ttu-id="964b7-267">Der Name der Tabellenspalte, deren Wert zur Auswertung der Bedingung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-267">The name of the table column whose value is used to evaluate the condition.</span></span>                                                                                                                                                                                                                   |
| <span data-ttu-id="964b7-268">**IsNull**</span><span class="sxs-lookup"><span data-stu-id="964b7-268">**IsNull**</span></span>     | <span data-ttu-id="964b7-269">Nein</span><span class="sxs-lookup"><span data-stu-id="964b7-269">No</span></span>          | <span data-ttu-id="964b7-270">**"True"** oder **"false"**.</span><span class="sxs-lookup"><span data-stu-id="964b7-270">**True** or **False**.</span></span> <span data-ttu-id="964b7-271">Wenn der Wert ist **"true"** und der Spaltenwert **null**, oder wenn der Wert ist **"false"** und der Wert der Spalte nicht **null**, die Bedingung wahr ist .</span><span class="sxs-lookup"><span data-stu-id="964b7-271">If the value is **True** and the column value is **null**, or if the value is **False** and the column value is not **null**, the condition is true.</span></span> <span data-ttu-id="964b7-272">Andernfalls ist die Bedingung nicht erfüllt (false).</span><span class="sxs-lookup"><span data-stu-id="964b7-272">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="964b7-273">Die **IsNull** und **Wert** Attribute können nicht gleichzeitig verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-273">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span> |
| <span data-ttu-id="964b7-274">**Wert**</span><span class="sxs-lookup"><span data-stu-id="964b7-274">**Value**</span></span>      | <span data-ttu-id="964b7-275">Nein</span><span class="sxs-lookup"><span data-stu-id="964b7-275">No</span></span>          | <span data-ttu-id="964b7-276">Der Wert, mit dem der Spaltenwert verglichen werden soll.</span><span class="sxs-lookup"><span data-stu-id="964b7-276">The value with which the column value is compared.</span></span> <span data-ttu-id="964b7-277">Wenn die Werte gleich sind, wird die Bedingung erfüllt (true).</span><span class="sxs-lookup"><span data-stu-id="964b7-277">If the values are the same, the condition is true.</span></span> <span data-ttu-id="964b7-278">Andernfalls ist die Bedingung nicht erfüllt (false).</span><span class="sxs-lookup"><span data-stu-id="964b7-278">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="964b7-279">Die **IsNull** und **Wert** Attribute können nicht gleichzeitig verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-279">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span>                                                                       |
| <span data-ttu-id="964b7-280">**Name**</span><span class="sxs-lookup"><span data-stu-id="964b7-280">**Name**</span></span>       | <span data-ttu-id="964b7-281">Nein</span><span class="sxs-lookup"><span data-stu-id="964b7-281">No</span></span>          | <span data-ttu-id="964b7-282">Der Name der Entitätseigenschaft im konzeptionellen Modell, deren Wert zur Auswertung der Bedingung verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-282">The name of the conceptual model entity property whose value is used to evaluate the condition.</span></span> <br/> <span data-ttu-id="964b7-283">Dieses Attribut gilt nicht wenn die **Bedingung** Element innerhalb eines FunctionImportMapping-Elements verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-283">This attribute is not applicable if the **Condition** element is used within a FunctionImportMapping element.</span></span>                                                                           |

### <a name="example"></a><span data-ttu-id="964b7-284">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-284">Example</span></span>

<span data-ttu-id="964b7-285">Das folgende Beispiel zeigt **Bedingung** Elemente als untergeordnete Elemente des **MappingFragment** Elemente.</span><span class="sxs-lookup"><span data-stu-id="964b7-285">The following example shows **Condition** elements as children of **MappingFragment** elements.</span></span> <span data-ttu-id="964b7-286">Wenn **HireDate** ist nicht null und **EnrollmentDate** ist null ist, Daten zwischen zugeordnet der **SchoolModel.Instructor** Typ und die **PersonID**und **HireDate** Spalten der **Person** Tabelle.</span><span class="sxs-lookup"><span data-stu-id="964b7-286">When **HireDate** is not null and **EnrollmentDate** is null, data is mapped between the **SchoolModel.Instructor** type and the **PersonID** and **HireDate** columns of the **Person** table.</span></span> <span data-ttu-id="964b7-287">Wenn **EnrollmentDate** ist nicht null und **HireDate** ist null ist, Daten zwischen zugeordnet der **SchoolModel.Student** Typ und die **PersonID** und **Registrierung** Spalten der **Person** Tabelle.</span><span class="sxs-lookup"><span data-stu-id="964b7-287">When **EnrollmentDate** is not null and **HireDate** is null, data is mapped between the **SchoolModel.Student** type and the **PersonID** and **Enrollment** columns of the **Person** table.</span></span>

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

## <a name="deletefunction-element-msl"></a><span data-ttu-id="964b7-288">DeleteFunction-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="964b7-288">DeleteFunction Element (MSL)</span></span>

<span data-ttu-id="964b7-289">Die **DeleteFunction** -Element der mapping-Spezifikationssprache (MSL) ordnet die Löschfunktion eines Entitätstyps oder einer Zuordnung im konzeptionellen Modell einer gespeicherten Prozedur in der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="964b7-289">The **DeleteFunction** element in mapping specification language (MSL) maps the delete function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="964b7-290">Gespeicherte Prozeduren, denen Änderungsfunktionen zugeordnet werden, müssen im Speichermodell deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-290">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="964b7-291">Weitere Informationen finden Sie in der Function-Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="964b7-291">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="964b7-292">Wenn Sie nicht zuordnen alle drei Einfüge-, update oder delete-Vorgänge eines Entitätstyps zu gespeicherten Prozeduren, wird die nicht zugeordneten Vorgänge fehl, wenn zur Laufzeit ausgeführt und ein "UpdateException" ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="964b7-292">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

### <a name="deletefunction-applied-to-entitytypemapping"></a><span data-ttu-id="964b7-293">DeleteFunction angewendet auf EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="964b7-293">DeleteFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="964b7-294">Bei Anwendung auf die EntityTypeMapping-Element, das **DeleteFunction** Element wird die Löschfunktion eines Entitätstyps im konzeptionellen Modell einer gespeicherten Prozedur zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="964b7-294">When applied to the EntityTypeMapping element, the **DeleteFunction** element maps the delete function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="964b7-295">Die **DeleteFunction** Element haben die folgenden untergeordneten Elemente bei Anwendung auf eine **EntityTypeMapping** Element:</span><span class="sxs-lookup"><span data-stu-id="964b7-295">The **DeleteFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="964b7-296">AssociationEnd (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-296">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="964b7-297">ComplexProperty (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-297">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="964b7-298">ScalarProperty (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-298">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="964b7-299">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="964b7-299">Applicable Attributes</span></span>

<span data-ttu-id="964b7-300">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **DeleteFunction** -Element, wenn es gilt eine **EntityTypeMapping** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-300">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="964b7-301">Attributname</span><span class="sxs-lookup"><span data-stu-id="964b7-301">Attribute Name</span></span>            | <span data-ttu-id="964b7-302">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="964b7-302">Is Required</span></span> | <span data-ttu-id="964b7-303">Wert</span><span class="sxs-lookup"><span data-stu-id="964b7-303">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="964b7-304">**Funktionsname**</span><span class="sxs-lookup"><span data-stu-id="964b7-304">**FunctionName**</span></span>          | <span data-ttu-id="964b7-305">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-305">Yes</span></span>         | <span data-ttu-id="964b7-306">Der mit einem Namespace qualifizierte Name der gespeicherten Prozedur, der die Löschfunktion zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-306">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="964b7-307">Die gespeicherte Prozedur muss im Speichermodell deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-307">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="964b7-308">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="964b7-308">**RowsAffectedParameter**</span></span> | <span data-ttu-id="964b7-309">Nein</span><span class="sxs-lookup"><span data-stu-id="964b7-309">No</span></span>          | <span data-ttu-id="964b7-310">Der Name des Ausgabeparameters, der die Anzahl der betroffenen Zeilen zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="964b7-310">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="964b7-311">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-311">Example</span></span>

<span data-ttu-id="964b7-312">Das folgende Beispiel basiert auf dem Modell "School" und zeigt die **DeleteFunction** -Element die Löschfunktion Zuordnen der **Person** Entitätstyp, der die **DeletePerson** gespeicherte Prozedur.</span><span class="sxs-lookup"><span data-stu-id="964b7-312">The following example is based on the School model and shows the **DeleteFunction** element mapping the delete function of the **Person** entity type to the **DeletePerson** stored procedure.</span></span> <span data-ttu-id="964b7-313">Die **DeletePerson** wird die gespeicherte Prozedur im Speichermodell deklariert.</span><span class="sxs-lookup"><span data-stu-id="964b7-313">The **DeletePerson** stored procedure is declared in the storage model.</span></span>

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

### <a name="deletefunction-applied-to-associationsetmapping"></a><span data-ttu-id="964b7-314">DeleteFunction angewendet auf AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="964b7-314">DeleteFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="964b7-315">Bei Anwendung auf das AssociationSetMapping-Element, das **DeleteFunction** Element wird die Löschfunktion einer Zuordnung im konzeptionellen Modell einer gespeicherten Prozedur zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="964b7-315">When applied to the AssociationSetMapping element, the **DeleteFunction** element maps the delete function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="964b7-316">Die **DeleteFunction** Element haben die folgenden untergeordneten Elemente bei Anwendung auf die **AssociationSetMapping** Element:</span><span class="sxs-lookup"><span data-stu-id="964b7-316">The **DeleteFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="964b7-317">EndProperty</span><span class="sxs-lookup"><span data-stu-id="964b7-317">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="964b7-318">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="964b7-318">Applicable Attributes</span></span>

<span data-ttu-id="964b7-319">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **DeleteFunction** -Element, wenn es gilt der **AssociationSetMapping** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-319">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="964b7-320">Attributname</span><span class="sxs-lookup"><span data-stu-id="964b7-320">Attribute Name</span></span>            | <span data-ttu-id="964b7-321">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="964b7-321">Is Required</span></span> | <span data-ttu-id="964b7-322">Wert</span><span class="sxs-lookup"><span data-stu-id="964b7-322">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="964b7-323">**Funktionsname**</span><span class="sxs-lookup"><span data-stu-id="964b7-323">**FunctionName**</span></span>          | <span data-ttu-id="964b7-324">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-324">Yes</span></span>         | <span data-ttu-id="964b7-325">Der mit einem Namespace qualifizierte Name der gespeicherten Prozedur, der die Löschfunktion zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-325">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="964b7-326">Die gespeicherte Prozedur muss im Speichermodell deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-326">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="964b7-327">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="964b7-327">**RowsAffectedParameter**</span></span> | <span data-ttu-id="964b7-328">Nein</span><span class="sxs-lookup"><span data-stu-id="964b7-328">No</span></span>          | <span data-ttu-id="964b7-329">Der Name des Ausgabeparameters, der die Anzahl der betroffenen Zeilen zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="964b7-329">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="964b7-330">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-330">Example</span></span>

<span data-ttu-id="964b7-331">Das folgende Beispiel basiert auf dem Modell "School" und zeigt die **DeleteFunction** Element verwendet, um die Löschfunktion Zuordnen der **CourseInstructor** Zuordnung, die die  **DeleteCourseInstructor** gespeicherte Prozedur.</span><span class="sxs-lookup"><span data-stu-id="964b7-331">The following example is based on the School model and shows the **DeleteFunction** element used to map delete function of the **CourseInstructor** association to the **DeleteCourseInstructor** stored procedure.</span></span> <span data-ttu-id="964b7-332">Die **DeleteCourseInstructor** wird die gespeicherte Prozedur im Speichermodell deklariert.</span><span class="sxs-lookup"><span data-stu-id="964b7-332">The **DeleteCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="endproperty-element-msl"></a><span data-ttu-id="964b7-333">EndProperty-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="964b7-333">EndProperty Element (MSL)</span></span>

<span data-ttu-id="964b7-334">Die **EndProperty** -Element der mapping-Spezifikationssprache (MSL) definiert die Zuordnung eines Endes oder einer Änderungsfunktion einer konzeptionellen Modellzuordnung zu der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="964b7-334">The **EndProperty** element in mapping specification language (MSL) defines the mapping between an end or a modification function of a conceptual model association and the underlying database.</span></span> <span data-ttu-id="964b7-335">Die Eigenschaftenspalte Zuordnung wird in einem untergeordneten ScalarProperty-Element angegeben.</span><span class="sxs-lookup"><span data-stu-id="964b7-335">The property-column mapping is specified in a child ScalarProperty element.</span></span>

<span data-ttu-id="964b7-336">Wenn ein **EndProperty** Element wird verwendet, um die Zuordnung für das Ende einer konzeptionellen Modellzuordnung zu definieren, es ist ein untergeordnetes Element eines AssociationSetMapping-Elements.</span><span class="sxs-lookup"><span data-stu-id="964b7-336">When an **EndProperty** element is used to define the mapping for the end of a conceptual model association, it is a child of an AssociationSetMapping element.</span></span> <span data-ttu-id="964b7-337">Wenn die **EndProperty** Element wird verwendet, um die Zuordnung für eine Änderungsfunktion einer konzeptionellen Modellzuordnung zu definieren, es ist ein untergeordnetes Element eines InsertFunction-Element oder DeleteFunction-Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-337">When the **EndProperty** element is used to define the mapping for a modification function of a conceptual model association, it is a child of an InsertFunction element or DeleteFunction element.</span></span>

<span data-ttu-id="964b7-338">Die **EndProperty** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="964b7-338">The **EndProperty** element can have the following child elements:</span></span>

-   <span data-ttu-id="964b7-339">ScalarProperty (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-339">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="964b7-340">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="964b7-340">Applicable Attributes</span></span>

<span data-ttu-id="964b7-341">Die folgende Tabelle beschreibt die Attribute für die **EndProperty** Element:</span><span class="sxs-lookup"><span data-stu-id="964b7-341">The following table describes the attributes that are applicable to the **EndProperty** element:</span></span>

| <span data-ttu-id="964b7-342">Attributname</span><span class="sxs-lookup"><span data-stu-id="964b7-342">Attribute Name</span></span> | <span data-ttu-id="964b7-343">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="964b7-343">Is Required</span></span> | <span data-ttu-id="964b7-344">Wert</span><span class="sxs-lookup"><span data-stu-id="964b7-344">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="964b7-345">name</span><span class="sxs-lookup"><span data-stu-id="964b7-345">Name</span></span>           | <span data-ttu-id="964b7-346">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-346">Yes</span></span>         | <span data-ttu-id="964b7-347">Der Name des Zuordnungsendes, das zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-347">The name of the association end that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="964b7-348">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-348">Example</span></span>

<span data-ttu-id="964b7-349">Das folgende Beispiel zeigt eine **AssociationSetMapping** Element, in dem die **FK\_Kurs\_Abteilung** der ZuordnungimkonzeptionellenModellzugeordnetist**Kurs** Tabelle in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="964b7-349">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="964b7-350">Zuordnungen zwischen zuordnungstypeigenschaften zu Tabellenspalten werden in untergeordneten angegeben **EndProperty** Elemente.</span><span class="sxs-lookup"><span data-stu-id="964b7-350">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

### <a name="example"></a><span data-ttu-id="964b7-351">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-351">Example</span></span>

<span data-ttu-id="964b7-352">Das folgende Beispiel zeigt die **EndProperty** Element mapping einer Zuordnung der INSERT- und Delete-Funktionen (**CourseInstructor**) gespeicherten Prozeduren in der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="964b7-352">The following example shows the **EndProperty** element mapping the insert and delete functions of an association (**CourseInstructor**) to stored procedures in the underlying database.</span></span> <span data-ttu-id="964b7-353">Die Funktionen, denen sie zugeordnet werden, sind im Speichermodell deklariert.</span><span class="sxs-lookup"><span data-stu-id="964b7-353">The functions that are mapped to are declared in the storage model.</span></span>

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

## <a name="entitycontainermapping-element-msl"></a><span data-ttu-id="964b7-354">EntityContainerMapping-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="964b7-354">EntityContainerMapping Element (MSL)</span></span>

<span data-ttu-id="964b7-355">Die **EntityContainerMapping** -Element der mapping-Spezifikationssprache (MSL) ordnet den Entitätencontainer im konzeptionellen Modell der Entitätscontainer im Speichermodell.</span><span class="sxs-lookup"><span data-stu-id="964b7-355">The **EntityContainerMapping** element in mapping specification language (MSL) maps the entity container in the conceptual model to the entity container in the storage model.</span></span> <span data-ttu-id="964b7-356">Die **EntityContainerMapping** -Element der Mapping-Element untergeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="964b7-356">The **EntityContainerMapping** element is a child of the Mapping element.</span></span>

<span data-ttu-id="964b7-357">Die **EntityContainerMapping** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):</span><span class="sxs-lookup"><span data-stu-id="964b7-357">The **EntityContainerMapping** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="964b7-358">EntitySetMapping (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-358">EntitySetMapping (zero or more)</span></span>
-   <span data-ttu-id="964b7-359">AssociationSetMapping (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-359">AssociationSetMapping (zero or more)</span></span>
-   <span data-ttu-id="964b7-360">FunctionImportMapping (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-360">FunctionImportMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="964b7-361">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="964b7-361">Applicable Attributes</span></span>

<span data-ttu-id="964b7-362">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **EntityContainerMapping** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-362">The following table describes the attributes that can be applied to the **EntityContainerMapping** element.</span></span>

| <span data-ttu-id="964b7-363">Attributname</span><span class="sxs-lookup"><span data-stu-id="964b7-363">Attribute Name</span></span>            | <span data-ttu-id="964b7-364">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="964b7-364">Is Required</span></span> | <span data-ttu-id="964b7-365">Wert</span><span class="sxs-lookup"><span data-stu-id="964b7-365">Value</span></span>                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="964b7-366">**StorageModelContainer**</span><span class="sxs-lookup"><span data-stu-id="964b7-366">**StorageModelContainer**</span></span> | <span data-ttu-id="964b7-367">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-367">Yes</span></span>         | <span data-ttu-id="964b7-368">Der Name des Entitätscontainers im Speichermodell, der zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-368">The name of the storage model entity container that is being mapped.</span></span>                                                                                                                                                                                     |
| <span data-ttu-id="964b7-369">**CdmEntityContainer**</span><span class="sxs-lookup"><span data-stu-id="964b7-369">**CdmEntityContainer**</span></span>    | <span data-ttu-id="964b7-370">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-370">Yes</span></span>         | <span data-ttu-id="964b7-371">Der Name des Entitätscontainers im konzeptionellen Modell, der zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-371">The name of the conceptual model entity container that is being mapped.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="964b7-372">**GenerateUpdateViews**</span><span class="sxs-lookup"><span data-stu-id="964b7-372">**GenerateUpdateViews**</span></span>   | <span data-ttu-id="964b7-373">Nein</span><span class="sxs-lookup"><span data-stu-id="964b7-373">No</span></span>          | <span data-ttu-id="964b7-374">**"True"** oder **"false"**.</span><span class="sxs-lookup"><span data-stu-id="964b7-374">**True** or **False**.</span></span> <span data-ttu-id="964b7-375">Wenn **"false"**, es werden keine Updateansichten generiert.</span><span class="sxs-lookup"><span data-stu-id="964b7-375">If **False**, no update views are generated.</span></span> <span data-ttu-id="964b7-376">Dieses Attribut sollte festgelegt werden, um **"false"** Wenn Sie eine schreibgeschützte Zuordnung, die ungültig wäre, da Daten möglicherweise erfolgreich Fällen kein Roundtrip ausgeführt haben.</span><span class="sxs-lookup"><span data-stu-id="964b7-376">This attribute should be set to **False** when you have a read-only mapping that would be invalid because data may not round-trip successfully.</span></span> <br/> <span data-ttu-id="964b7-377">Der Standardwert ist **"true"**.</span><span class="sxs-lookup"><span data-stu-id="964b7-377">The default value is **True**.</span></span> |

### <a name="example"></a><span data-ttu-id="964b7-378">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-378">Example</span></span>

<span data-ttu-id="964b7-379">Das folgende Beispiel zeigt eine **EntityContainerMapping** Element, zugeordnet ist, die **SchoolModelEntities** Container (der Entitätscontainer des konzeptionellen Modells), die  **SchoolModelStoreContainer** Container (das Speichermodell-Entitätscontainer):</span><span class="sxs-lookup"><span data-stu-id="964b7-379">The following example shows an **EntityContainerMapping** element that maps the **SchoolModelEntities** container (the conceptual model entity container) to the **SchoolModelStoreContainer** container (the storage model entity container):</span></span>

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

## <a name="entitysetmapping-element-msl"></a><span data-ttu-id="964b7-380">EntitySetMapping-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="964b7-380">EntitySetMapping Element (MSL)</span></span>

<span data-ttu-id="964b7-381">Die **EntitySetMapping** -Element in mapping Specification Language (MSL) ordnet alle Typen in einem konzeptionellen modellentität auf der Entität im Speichermodell legt.</span><span class="sxs-lookup"><span data-stu-id="964b7-381">The **EntitySetMapping** element in mapping specification language (MSL) maps all types in a conceptual model entity set to entity sets in the storage model.</span></span> <span data-ttu-id="964b7-382">Eine Entitätenmenge im konzeptionellen Modell ist ein logischer Container für Instanzen der Entitäten des gleichen Typs (und abgeleitete Typen).</span><span class="sxs-lookup"><span data-stu-id="964b7-382">An entity set in the conceptual model is a logical container for instances of entities of the same type (and derived types).</span></span> <span data-ttu-id="964b7-383">Eine Entitätenmenge im Speichermodell stellt eine Tabelle oder Sicht in der zugrunde liegenden Datenbank dar.</span><span class="sxs-lookup"><span data-stu-id="964b7-383">An entity set in the storage model represents a table or view in the underlying database.</span></span> <span data-ttu-id="964b7-384">Entitätenmenge im konzeptionellen Modell wird angegeben, indem der Wert des der **Namen** Attribut der **EntitySetMapping** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-384">The conceptual model entity set is specified by the value of the **Name** attribute of the **EntitySetMapping** element.</span></span> <span data-ttu-id="964b7-385">Die zugeordnete Tabelle oder Sicht angegeben ist, indem die **StoreEntitySet** Attributs in jedem untergeordneten MappingFragment-Element oder in der **EntitySetMapping** Element selbst.</span><span class="sxs-lookup"><span data-stu-id="964b7-385">The mapped-to table or view is specified by the **StoreEntitySet** attribute in each child MappingFragment element or in the **EntitySetMapping** element itself.</span></span>

<span data-ttu-id="964b7-386">Die **EntitySetMapping** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="964b7-386">The **EntitySetMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="964b7-387">EntityTypeMapping (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-387">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="964b7-388">QueryView (0 (null) oder 1)</span><span class="sxs-lookup"><span data-stu-id="964b7-388">QueryView (zero or one)</span></span>
-   <span data-ttu-id="964b7-389">MappingFragment (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-389">MappingFragment (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="964b7-390">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="964b7-390">Applicable Attributes</span></span>

<span data-ttu-id="964b7-391">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **EntitySetMapping** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-391">The following table describes the attributes that can be applied to the **EntitySetMapping** element.</span></span>

| <span data-ttu-id="964b7-392">Attributname</span><span class="sxs-lookup"><span data-stu-id="964b7-392">Attribute Name</span></span>           | <span data-ttu-id="964b7-393">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="964b7-393">Is Required</span></span> | <span data-ttu-id="964b7-394">Wert</span><span class="sxs-lookup"><span data-stu-id="964b7-394">Value</span></span>                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="964b7-395">**Name**</span><span class="sxs-lookup"><span data-stu-id="964b7-395">**Name**</span></span>                 | <span data-ttu-id="964b7-396">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-396">Yes</span></span>         | <span data-ttu-id="964b7-397">Der Name der Entitätenmenge im konzeptionellen Modell, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-397">The name of the conceptual model entity set that is being mapped.</span></span>                                                                                                                                                             |
| <span data-ttu-id="964b7-398">**TypeName** **1**</span><span class="sxs-lookup"><span data-stu-id="964b7-398">**TypeName** **1**</span></span>       | <span data-ttu-id="964b7-399">Nein</span><span class="sxs-lookup"><span data-stu-id="964b7-399">No</span></span>          | <span data-ttu-id="964b7-400">Der Name des Entitätstyp im konzeptionellen Modell, der zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-400">The name of the conceptual model entity type that is being mapped.</span></span>                                                                                                                                                            |
| <span data-ttu-id="964b7-401">**StoreEntitySet** **1**</span><span class="sxs-lookup"><span data-stu-id="964b7-401">**StoreEntitySet** **1**</span></span> | <span data-ttu-id="964b7-402">Nein</span><span class="sxs-lookup"><span data-stu-id="964b7-402">No</span></span>          | <span data-ttu-id="964b7-403">Der Name der Entitätenmenge im Speichermodell, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-403">The name of the storage model entity set that is being mapped to.</span></span>                                                                                                                                                             |
| <span data-ttu-id="964b7-404">**Das MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="964b7-404">**MakeColumnsDistinct**</span></span>  | <span data-ttu-id="964b7-405">Nein</span><span class="sxs-lookup"><span data-stu-id="964b7-405">No</span></span>          | <span data-ttu-id="964b7-406">**"True"** oder **"false"** abhängig davon, ob nur unterschiedliche Zeilen zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-406">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="964b7-407">Wenn dieses Attribut, um festgelegt wird **"true"**, **GenerateUpdateViews** Attribut des EntityContainerMapping-Elements muss festgelegt werden, um **"false"**.</span><span class="sxs-lookup"><span data-stu-id="964b7-407">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

 

<span data-ttu-id="964b7-408">**1** der **TypeName** und **StoreEntitySet** Attribute können anstelle der EntityTypeMapping- und MappingFragment untergeordnete Elemente verwendet werden, um eine einzelne Tabelle einen einzelnen Entitätstyp zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="964b7-408">**1** The **TypeName** and **StoreEntitySet** attributes can be used in place of the EntityTypeMapping and MappingFragment child elements to map a single entity type to a single table.</span></span>

### <a name="example"></a><span data-ttu-id="964b7-409">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-409">Example</span></span>

<span data-ttu-id="964b7-410">Das folgende Beispiel zeigt eine **EntitySetMapping** -Element, das in drei Typen (Basistyp und zwei abgeleitete Typen) zugeordnet ist die **Kurse** Entitätenmenge des konzeptionellen Modells drei verschiedenen Tabellen in der zugrunde liegende Datenbank.</span><span class="sxs-lookup"><span data-stu-id="964b7-410">The following example shows an **EntitySetMapping** element that maps three types (a base type and two derived types) in the **Courses** entity set of the conceptual model to three different tables in the underlying database.</span></span> <span data-ttu-id="964b7-411">Die Tabellen werden angegeben, indem die **StoreEntitySet** -Attribut in jedem **MappingFragment** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-411">The tables are specified by the **StoreEntitySet** attribute in each **MappingFragment** element.</span></span>

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

## <a name="entitytypemapping-element-msl"></a><span data-ttu-id="964b7-412">EntityTypeMapping-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="964b7-412">EntityTypeMapping Element (MSL)</span></span>

<span data-ttu-id="964b7-413">Die **EntityTypeMapping** -Element der mapping-Spezifikationssprache (MSL) definiert die Zuordnung zwischen einen Entitätstyp im konzeptionellen Modell und Tabellen oder Sichten in der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="964b7-413">The **EntityTypeMapping** element in mapping specification language (MSL) defines the mapping between an entity type in the conceptual model and tables or views in the underlying database.</span></span> <span data-ttu-id="964b7-414">Informationen zu den Entitätstypen des konzeptionellen Modells und zugrunde liegenden Datenbanktabellen oder-Ansichten finden Sie unter EntityType-Element (CSDL) und der EntitySet-Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="964b7-414">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="964b7-415">Wird der Typ des konzeptionellen Modells Entität, die zugeordnet wird angegeben, indem die **TypeName** Attribut der **EntityTypeMapping** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-415">The conceptual model entity type that is being mapped is specified by the **TypeName** attribute of the **EntityTypeMapping** element.</span></span> <span data-ttu-id="964b7-416">Der Tabelle oder Sicht, die zugeordnet wird, wird angegeben durch die **StoreEntitySet** Attribut des untergeordneten MappingFragment-Elements.</span><span class="sxs-lookup"><span data-stu-id="964b7-416">The table or view that is being mapped is specified by the **StoreEntitySet** attribute of the child MappingFragment element.</span></span>

<span data-ttu-id="964b7-417">Das untergeordnete Element kann verwendet werden, zum Zuordnen von Einfüge-, ModificationFunctionMapping aktualisieren oder Löschfunktionen von Entitätstypen zu gespeicherten Prozeduren in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="964b7-417">The ModificationFunctionMapping child element can be used to map the insert, update, or delete functions of entity types to stored procedures in the database.</span></span>

<span data-ttu-id="964b7-418">Die **EntityTypeMapping** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="964b7-418">The **EntityTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="964b7-419">MappingFragment (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-419">MappingFragment (zero or more)</span></span>
-   <span data-ttu-id="964b7-420">ModificationFunctionMapping (0 (null) oder 1)</span><span class="sxs-lookup"><span data-stu-id="964b7-420">ModificationFunctionMapping (zero or one)</span></span>
-   <span data-ttu-id="964b7-421">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="964b7-421">ScalarProperty</span></span>
-   <span data-ttu-id="964b7-422">Bedingung</span><span class="sxs-lookup"><span data-stu-id="964b7-422">Condition</span></span>

> [!NOTE]
> <span data-ttu-id="964b7-423">**MappingFragment** und **ModificationFunctionMapping** Elementen handelt es sich nicht um untergeordnete Elemente der **EntityTypeMapping** Element zur gleichen Zeit.</span><span class="sxs-lookup"><span data-stu-id="964b7-423">**MappingFragment** and **ModificationFunctionMapping** elements cannot be child elements of the **EntityTypeMapping** element at the same time.</span></span>


> [!NOTE]
> <span data-ttu-id="964b7-424">Die **ScalarProperty** und **Bedingung** Elemente können nur untergeordnete Elemente werden der **EntityTypeMapping** -Element, wenn er in ein FunctionImportMapping-Element verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-424">The **ScalarProperty** and **Condition** elements can only be child elements of the **EntityTypeMapping** element when it is used within a FunctionImportMapping element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="964b7-425">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="964b7-425">Applicable Attributes</span></span>

<span data-ttu-id="964b7-426">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **EntityTypeMapping** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-426">The following table describes the attributes that can be applied to the **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="964b7-427">Attributname</span><span class="sxs-lookup"><span data-stu-id="964b7-427">Attribute Name</span></span> | <span data-ttu-id="964b7-428">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="964b7-428">Is Required</span></span> | <span data-ttu-id="964b7-429">Wert</span><span class="sxs-lookup"><span data-stu-id="964b7-429">Value</span></span>                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="964b7-430">**Typname**</span><span class="sxs-lookup"><span data-stu-id="964b7-430">**TypeName**</span></span>   | <span data-ttu-id="964b7-431">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-431">Yes</span></span>         | <span data-ttu-id="964b7-432">Der mit einem Namespace qualifizierte Name des Entitätstyps des konzeptionellen Modells, der zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-432">The namespace-qualified name of the conceptual model entity type that is being mapped.</span></span> <br/> <span data-ttu-id="964b7-433">Wenn der Typ abstrakt oder ein abgeleiteter Typ ist, muss der Wert `IsOfType(Namespace-qualified_type_name)` lauten.</span><span class="sxs-lookup"><span data-stu-id="964b7-433">If the type is abstract or a derived type, the value must be `IsOfType(Namespace-qualified_type_name)`.</span></span> |

### <a name="example"></a><span data-ttu-id="964b7-434">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-434">Example</span></span>

<span data-ttu-id="964b7-435">Das folgende Beispiel zeigt ein EntitySetMapping-Element mit zwei untergeordneten **EntityTypeMapping** Elemente.</span><span class="sxs-lookup"><span data-stu-id="964b7-435">The following example shows an EntitySetMapping element with two child **EntityTypeMapping** elements.</span></span> <span data-ttu-id="964b7-436">In der ersten **EntityTypeMapping** -Element, das **SchoolModel.Person** Entitätstyp zugeordnet ist die **Person** Tabelle.</span><span class="sxs-lookup"><span data-stu-id="964b7-436">In the first **EntityTypeMapping** element, the **SchoolModel.Person** entity type is mapped to the **Person** table.</span></span> <span data-ttu-id="964b7-437">In der zweiten **EntityTypeMapping** -Element, das die Aktualisierungsfunktion der **SchoolModel.Person** Typ einer gespeicherten Prozedur zugeordnet ist **UpdatePerson**, in der Datenbank .</span><span class="sxs-lookup"><span data-stu-id="964b7-437">In the second **EntityTypeMapping** element, the update functionality of the **SchoolModel.Person** type is mapped to a stored procedure, **UpdatePerson**, in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="964b7-438">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-438">Example</span></span>

<span data-ttu-id="964b7-439">Im nächsten Beispiel wird die Zuordnung einer Typhierarchie, in der der Stammtyp abstrakt ist, veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="964b7-439">The next example shows the mapping of a type hierarchy in which the root type is abstract.</span></span> <span data-ttu-id="964b7-440">Beachten Sie die Verwendung der `IsOfType` Syntax für die **TypeName** Attribute.</span><span class="sxs-lookup"><span data-stu-id="964b7-440">Note the use of the `IsOfType` syntax for the **TypeName** attributes.</span></span>

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

## <a name="functionimportmapping-element-msl"></a><span data-ttu-id="964b7-441">FunctionImportMapping-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="964b7-441">FunctionImportMapping Element (MSL)</span></span>

<span data-ttu-id="964b7-442">Die **FunctionImportMapping** -Element der mapping-Spezifikationssprache (MSL) definiert die Zuordnung zwischen einem Funktionsimport im konzeptionellen Modell und einer gespeicherten Prozedur oder Funktion in der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="964b7-442">The **FunctionImportMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure or function in the underlying database.</span></span> <span data-ttu-id="964b7-443">Funktionsimporte müssen im konzeptionellen Modell und gespeicherte Prozeduren müssen im Speichermodell deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-443">Function imports must be declared in the conceptual model and stored procedures must be declared in the storage model.</span></span> <span data-ttu-id="964b7-444">Weitere Informationen finden Sie unter FunctionImport-Element (CSDL) und Function-Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="964b7-444">For more information, see FunctionImport Element (CSDL) and Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="964b7-445">Wenn ein Funktionsimport einen Entitätstyp des konzeptionellen Modells oder einen komplexen Typ zurückgibt, dann müssen standardmäßig die Namen der Spalten, die von der zugrunde liegenden gespeicherten Prozedur zurückgegeben werden, exakt den Namen der Eigenschaften des konzeptionellen Modelltyps entsprechen.</span><span class="sxs-lookup"><span data-stu-id="964b7-445">By default, if a function import returns a conceptual model entity type or complex type, then the names of the columns returned by the underlying stored procedure must exactly match the names of the properties on the conceptual model type.</span></span> <span data-ttu-id="964b7-446">Wenn die Spaltennamen nicht genau mit die Eigenschaftennamen übereinstimmen, muss die Zuordnung in einem ResultMapping-Element definiert werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-446">If the column names do not exactly match the property names, the mapping must be defined in a ResultMapping element.</span></span>

<span data-ttu-id="964b7-447">Die **FunctionImportMapping** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="964b7-447">The **FunctionImportMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="964b7-448">ResultMapping (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-448">ResultMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="964b7-449">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="964b7-449">Applicable Attributes</span></span>

<span data-ttu-id="964b7-450">Die folgende Tabelle beschreibt die Attribute für die **FunctionImportMapping** Element:</span><span class="sxs-lookup"><span data-stu-id="964b7-450">The following table describes the attributes that are applicable to the **FunctionImportMapping** element:</span></span>

| <span data-ttu-id="964b7-451">Attributname</span><span class="sxs-lookup"><span data-stu-id="964b7-451">Attribute Name</span></span>         | <span data-ttu-id="964b7-452">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="964b7-452">Is Required</span></span> | <span data-ttu-id="964b7-453">Wert</span><span class="sxs-lookup"><span data-stu-id="964b7-453">Value</span></span>                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| <span data-ttu-id="964b7-454">**FunctionImportName**</span><span class="sxs-lookup"><span data-stu-id="964b7-454">**FunctionImportName**</span></span> | <span data-ttu-id="964b7-455">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-455">Yes</span></span>         | <span data-ttu-id="964b7-456">Der Name des Funktionsimports im konzeptionellen Modell, der zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-456">The name of the function import in the conceptual model that is being mapped.</span></span>           |
| <span data-ttu-id="964b7-457">**Funktionsname**</span><span class="sxs-lookup"><span data-stu-id="964b7-457">**FunctionName**</span></span>       | <span data-ttu-id="964b7-458">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-458">Yes</span></span>         | <span data-ttu-id="964b7-459">Der mit einem Namespace qualifizierte Name der Funktion im Speichermodell, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-459">The namespace-qualified name of the function in the storage model that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="964b7-460">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-460">Example</span></span>

<span data-ttu-id="964b7-461">Das folgende Beispiel basiert auf dem Modell "School".</span><span class="sxs-lookup"><span data-stu-id="964b7-461">The following example is based on the School model.</span></span> <span data-ttu-id="964b7-462">Betrachten Sie die folgende Funktion im Speichermodell:</span><span class="sxs-lookup"><span data-stu-id="964b7-462">Consider the following function in the storage model:</span></span>

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

<span data-ttu-id="964b7-463">Beachten Sie auch diesen Funktionsimport im konzeptionellen Modell:</span><span class="sxs-lookup"><span data-stu-id="964b7-463">Also consider this function import in the conceptual model:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

<span data-ttu-id="964b7-464">Das folgende Beispiel zeigt eine **FunctionImportMapping** , das zum Zuordnen der Funktion und der Funktionsimport oben miteinander verwendet:</span><span class="sxs-lookup"><span data-stu-id="964b7-464">The following example show a **FunctionImportMapping** element used to map the function and function import above to each other:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a><span data-ttu-id="964b7-465">InsertFunction-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="964b7-465">InsertFunction Element (MSL)</span></span>

<span data-ttu-id="964b7-466">Die **InsertFunction** -Element der mapping-Spezifikationssprache (MSL) ordnet die Einfügefunktion eines Entitätstyps oder einer Zuordnung im konzeptionellen Modell einer gespeicherten Prozedur in der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="964b7-466">The **InsertFunction** element in mapping specification language (MSL) maps the insert function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="964b7-467">Gespeicherte Prozeduren, denen Änderungsfunktionen zugeordnet werden, müssen im Speichermodell deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-467">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="964b7-468">Weitere Informationen finden Sie in der Function-Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="964b7-468">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="964b7-469">Wenn Sie nicht zuordnen alle drei Einfüge-, update oder delete-Vorgänge eines Entitätstyps zu gespeicherten Prozeduren, wird die nicht zugeordneten Vorgänge fehl, wenn zur Laufzeit ausgeführt und ein "UpdateException" ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="964b7-469">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="964b7-470">Die **InsertFunction** Element kann ein untergeordnetes Element des ModificationFunctionMapping-Element und angewendet auf EntityTypeMapping-Element oder die AssociationSetMapping-Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-470">The **InsertFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

### <a name="insertfunction-applied-to-entitytypemapping"></a><span data-ttu-id="964b7-471">InsertFunction angewendet auf EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="964b7-471">InsertFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="964b7-472">Bei Anwendung auf die EntityTypeMapping-Element, das **InsertFunction** -Element ordnet die Einfügefunktion eines Entitätstyps im konzeptionellen Modell einer gespeicherten Prozedur.</span><span class="sxs-lookup"><span data-stu-id="964b7-472">When applied to the EntityTypeMapping element, the **InsertFunction** element maps the insert function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="964b7-473">Die **InsertFunction** Element haben die folgenden untergeordneten Elemente bei Anwendung auf eine **EntityTypeMapping** Element:</span><span class="sxs-lookup"><span data-stu-id="964b7-473">The **InsertFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="964b7-474">AssociationEnd (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-474">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="964b7-475">ComplexProperty (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-475">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="964b7-476">ResultBinding (0 (null) oder 1)</span><span class="sxs-lookup"><span data-stu-id="964b7-476">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="964b7-477">ScalarProperty (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-477">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="964b7-478">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="964b7-478">Applicable Attributes</span></span>

<span data-ttu-id="964b7-479">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **InsertFunction** Element bei Anwendung auf eine **EntityTypeMapping** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-479">The following table describes the attributes that can be applied to the **InsertFunction** element when applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="964b7-480">Attributname</span><span class="sxs-lookup"><span data-stu-id="964b7-480">Attribute Name</span></span>            | <span data-ttu-id="964b7-481">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="964b7-481">Is Required</span></span> | <span data-ttu-id="964b7-482">Wert</span><span class="sxs-lookup"><span data-stu-id="964b7-482">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="964b7-483">**Funktionsname**</span><span class="sxs-lookup"><span data-stu-id="964b7-483">**FunctionName**</span></span>          | <span data-ttu-id="964b7-484">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-484">Yes</span></span>         | <span data-ttu-id="964b7-485">Der mit einem Namespace qualifizierte Name der gespeicherten Prozedur, der die Einfügefunktion zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-485">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="964b7-486">Die gespeicherte Prozedur muss im Speichermodell deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-486">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="964b7-487">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="964b7-487">**RowsAffectedParameter**</span></span> | <span data-ttu-id="964b7-488">Nein</span><span class="sxs-lookup"><span data-stu-id="964b7-488">No</span></span>          | <span data-ttu-id="964b7-489">Der Name des Ausgabeparameters, der die Anzahl der betroffenen Zeilen zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="964b7-489">The name of the output parameter that returns the number of affected rows.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="964b7-490">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-490">Example</span></span>

<span data-ttu-id="964b7-491">Das folgende Beispiel basiert auf dem Modell "School" und zeigt die **InsertFunction** Element verwendet, um die Einfügefunktion des Entitätstyps Person zum Zuordnen der **InsertPerson** gespeicherte Prozedur.</span><span class="sxs-lookup"><span data-stu-id="964b7-491">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the Person entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="964b7-492">Die **InsertPerson** wird die gespeicherte Prozedur im Speichermodell deklariert.</span><span class="sxs-lookup"><span data-stu-id="964b7-492">The **InsertPerson** stored procedure is declared in the storage model.</span></span>

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
### <a name="insertfunction-applied-to-associationsetmapping"></a><span data-ttu-id="964b7-493">InsertFunction angewendet auf AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="964b7-493">InsertFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="964b7-494">Bei Anwendung auf das AssociationSetMapping-Element, das **InsertFunction** -Element ordnet die Einfügefunktion einer Zuordnung im konzeptionellen Modell einer gespeicherten Prozedur.</span><span class="sxs-lookup"><span data-stu-id="964b7-494">When applied to the AssociationSetMapping element, the **InsertFunction** element maps the insert function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="964b7-495">Die **InsertFunction** Element haben die folgenden untergeordneten Elemente bei Anwendung auf die **AssociationSetMapping** Element:</span><span class="sxs-lookup"><span data-stu-id="964b7-495">The **InsertFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="964b7-496">EndProperty</span><span class="sxs-lookup"><span data-stu-id="964b7-496">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="964b7-497">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="964b7-497">Applicable Attributes</span></span>

<span data-ttu-id="964b7-498">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **InsertFunction** -Element, wenn es gilt der **AssociationSetMapping** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-498">The following table describes the attributes that can be applied to the **InsertFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="964b7-499">Attributname</span><span class="sxs-lookup"><span data-stu-id="964b7-499">Attribute Name</span></span>            | <span data-ttu-id="964b7-500">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="964b7-500">Is Required</span></span> | <span data-ttu-id="964b7-501">Wert</span><span class="sxs-lookup"><span data-stu-id="964b7-501">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="964b7-502">**Funktionsname**</span><span class="sxs-lookup"><span data-stu-id="964b7-502">**FunctionName**</span></span>          | <span data-ttu-id="964b7-503">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-503">Yes</span></span>         | <span data-ttu-id="964b7-504">Der mit einem Namespace qualifizierte Name der gespeicherten Prozedur, der die Einfügefunktion zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-504">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="964b7-505">Die gespeicherte Prozedur muss im Speichermodell deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-505">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="964b7-506">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="964b7-506">**RowsAffectedParameter**</span></span> | <span data-ttu-id="964b7-507">Nein</span><span class="sxs-lookup"><span data-stu-id="964b7-507">No</span></span>          | <span data-ttu-id="964b7-508">Der Name des Ausgabeparameters, der die Anzahl der betroffenen Zeilen zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="964b7-508">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="964b7-509">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-509">Example</span></span>

<span data-ttu-id="964b7-510">Das folgende Beispiel basiert auf dem Modell "School" und zeigt die **InsertFunction** Element verwendet, um die Einfügefunktion Zuordnen der **CourseInstructor** Zuordnung, die die  **InsertCourseInstructor** gespeicherte Prozedur.</span><span class="sxs-lookup"><span data-stu-id="964b7-510">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the **CourseInstructor** association to the **InsertCourseInstructor** stored procedure.</span></span> <span data-ttu-id="964b7-511">Die **InsertCourseInstructor** wird die gespeicherte Prozedur im Speichermodell deklariert.</span><span class="sxs-lookup"><span data-stu-id="964b7-511">The **InsertCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="mapping-element-msl"></a><span data-ttu-id="964b7-512">Mapping-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="964b7-512">Mapping Element (MSL)</span></span>

<span data-ttu-id="964b7-513">Die **Zuordnung** -Element der mapping-Spezifikationssprache (MSL) enthält Informationen zum Zuordnen von Objekten, die in einem konzeptionellen Modell mit einer Datenbank definiert sind (wie in einem Speichermodell beschrieben).</span><span class="sxs-lookup"><span data-stu-id="964b7-513">The **Mapping** element in mapping specification language (MSL) contains information for mapping objects that are defined in a conceptual model to a database (as described in a storage model).</span></span> <span data-ttu-id="964b7-514">Weitere Informationen finden Sie in der CSDL-Spezifikation und SSDL-Spezifikation.</span><span class="sxs-lookup"><span data-stu-id="964b7-514">For more information, see CSDL Specification and SSDL Specification.</span></span>

<span data-ttu-id="964b7-515">Die **Zuordnung** Element ist das Stammelement für die Mapping-Spezifikation.</span><span class="sxs-lookup"><span data-stu-id="964b7-515">The **Mapping** element is the root element for a mapping specification.</span></span> <span data-ttu-id="964b7-516">Der XML-Namespace für mapping-Spezifikationen http://schemas.microsoft.com/ado/2009/11/mapping/cs.</span><span class="sxs-lookup"><span data-stu-id="964b7-516">The XML namespace for mapping specifications is http://schemas.microsoft.com/ado/2009/11/mapping/cs.</span></span>

<span data-ttu-id="964b7-517">Das Mapping-Element kann die folgenden untergeordneten Elemente aufweisen (der vorliegenden Reihenfolge entsprechend):</span><span class="sxs-lookup"><span data-stu-id="964b7-517">The mapping element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="964b7-518">Alias (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-518">Alias (zero or more)</span></span>
-   <span data-ttu-id="964b7-519">EntityContainerMapping (genau ein Element)</span><span class="sxs-lookup"><span data-stu-id="964b7-519">EntityContainerMapping (exactly one)</span></span>

<span data-ttu-id="964b7-520">Die Namen aller Typen des konzeptionellen Modells und Typen des Speichermodells, auf die in MSL verwiesen wird, müssen mit dem jeweiligen Namespacenamen qualifiziert werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-520">Names of conceptual and storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="964b7-521">Informationen zu den Namespacenamen des konzeptionellen Modells finden Sie in der Schema-Element (CSDL).</span><span class="sxs-lookup"><span data-stu-id="964b7-521">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="964b7-522">Informationen zu den Namespacenamen des Storage-Modell finden Sie in der Schema-Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="964b7-522">For information about the storage model namespace name, see Schema Element (SSDL).</span></span> <span data-ttu-id="964b7-523">Aliasnamen für Namespaces, die in MSL verwendet werden, können mit dem Alias-Element definiert werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-523">Aliases for namespaces that are used in MSL can be defined with the Alias element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="964b7-524">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="964b7-524">Applicable Attributes</span></span>

<span data-ttu-id="964b7-525">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Zuordnung** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-525">The table below describes the attributes that can be applied to the **Mapping** element.</span></span>

| <span data-ttu-id="964b7-526">Attributname</span><span class="sxs-lookup"><span data-stu-id="964b7-526">Attribute Name</span></span> | <span data-ttu-id="964b7-527">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="964b7-527">Is Required</span></span> | <span data-ttu-id="964b7-528">Wert</span><span class="sxs-lookup"><span data-stu-id="964b7-528">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="964b7-529">**LEERTASTE**</span><span class="sxs-lookup"><span data-stu-id="964b7-529">**Space**</span></span>      | <span data-ttu-id="964b7-530">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-530">Yes</span></span>         | <span data-ttu-id="964b7-531">**C-S-**.</span><span class="sxs-lookup"><span data-stu-id="964b7-531">**C-S**.</span></span> <span data-ttu-id="964b7-532">Dies ist ein fester Wert, der nicht geändert werden kann.</span><span class="sxs-lookup"><span data-stu-id="964b7-532">This is a fixed value and cannot be changed.</span></span> |

### <a name="example"></a><span data-ttu-id="964b7-533">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-533">Example</span></span>

<span data-ttu-id="964b7-534">Das folgende Beispiel zeigt eine **Zuordnung** -Element, das Teil des Modells "School" basiert.</span><span class="sxs-lookup"><span data-stu-id="964b7-534">The following example shows a **Mapping** element that is based on part of the School model.</span></span> <span data-ttu-id="964b7-535">Weitere Informationen über das Modell "School" finden Sie in Schnellstart (Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="964b7-535">For more information about the School model, see Quickstart (Entity Framework):</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="http://schemas.microsoft.com/ado/2009/11/mapping/cs">
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

## <a name="mappingfragment-element-msl"></a><span data-ttu-id="964b7-536">MappingFragment-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="964b7-536">MappingFragment Element (MSL)</span></span>

<span data-ttu-id="964b7-537">Die **MappingFragment** -Element der mapping-Spezifikationssprache (MSL) definiert die Zuordnung zwischen den Eigenschaften einen Entitätstyp des konzeptionellen Modells und einer Tabelle oder Sicht in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="964b7-537">The **MappingFragment** element in mapping specification language (MSL) defines the mapping between the properties of a conceptual model entity type and a table or view in the database.</span></span> <span data-ttu-id="964b7-538">Informationen zu den Entitätstypen des konzeptionellen Modells und zugrunde liegenden Datenbanktabellen oder-Ansichten finden Sie unter EntityType-Element (CSDL) und der EntitySet-Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="964b7-538">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="964b7-539">Die **MappingFragment** kann ein untergeordnetes Element des EntityTypeMapping-Element oder das EntitySetMapping-Element sein.</span><span class="sxs-lookup"><span data-stu-id="964b7-539">The **MappingFragment** can be a child element of the EntityTypeMapping element or the EntitySetMapping element.</span></span>

<span data-ttu-id="964b7-540">Die **MappingFragment** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="964b7-540">The **MappingFragment** element can have the following child elements:</span></span>

-   <span data-ttu-id="964b7-541">ComplexType (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-541">ComplexType (zero or more)</span></span>
-   <span data-ttu-id="964b7-542">ScalarProperty (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-542">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="964b7-543">Bedingung (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-543">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="964b7-544">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="964b7-544">Applicable Attributes</span></span>

<span data-ttu-id="964b7-545">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **MappingFragment** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-545">The following table describes the attributes that can be applied to the **MappingFragment** element.</span></span>

| <span data-ttu-id="964b7-546">Attributname</span><span class="sxs-lookup"><span data-stu-id="964b7-546">Attribute Name</span></span>          | <span data-ttu-id="964b7-547">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="964b7-547">Is Required</span></span> | <span data-ttu-id="964b7-548">Wert</span><span class="sxs-lookup"><span data-stu-id="964b7-548">Value</span></span>                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="964b7-549">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="964b7-549">**StoreEntitySet**</span></span>      | <span data-ttu-id="964b7-550">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-550">Yes</span></span>         | <span data-ttu-id="964b7-551">Der Name der Tabelle oder Ansicht, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-551">The name of the table or view that is being mapped.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="964b7-552">**Das MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="964b7-552">**MakeColumnsDistinct**</span></span> | <span data-ttu-id="964b7-553">Nein</span><span class="sxs-lookup"><span data-stu-id="964b7-553">No</span></span>          | <span data-ttu-id="964b7-554">**"True"** oder **"false"** abhängig davon, ob nur unterschiedliche Zeilen zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-554">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="964b7-555">Wenn dieses Attribut, um festgelegt wird **"true"**, **GenerateUpdateViews** Attribut des EntityContainerMapping-Elements muss festgelegt werden, um **"false"**.</span><span class="sxs-lookup"><span data-stu-id="964b7-555">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

### <a name="example"></a><span data-ttu-id="964b7-556">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-556">Example</span></span>

<span data-ttu-id="964b7-557">Das folgende Beispiel zeigt eine **MappingFragment** Element als untergeordnetes Element eine **EntityTypeMapping** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-557">The following example shows a **MappingFragment** element as the child of an **EntityTypeMapping** element.</span></span> <span data-ttu-id="964b7-558">In diesem Beispiel ist die Eigenschaften der **Kurs** Spalten des Typs im konzeptionellen Modell zugeordnet sind die **Kurs** Tabelle in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="964b7-558">In this example, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="964b7-559">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-559">Example</span></span>

<span data-ttu-id="964b7-560">Das folgende Beispiel zeigt eine **MappingFragment** Element als untergeordnetes Element eine **EntitySetMapping** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-560">The following example shows a **MappingFragment** element as the child of an **EntitySetMapping** element.</span></span> <span data-ttu-id="964b7-561">Wie im Beispiel oben "," Eigenschaften der **Kurs** Spalten des Typs im konzeptionellen Modell zugeordnet sind die **Kurs** Tabelle in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="964b7-561">As in the example above, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

## <a name="modificationfunctionmapping-element-msl"></a><span data-ttu-id="964b7-562">ModificationFunctionMapping-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="964b7-562">ModificationFunctionMapping Element (MSL)</span></span>

<span data-ttu-id="964b7-563">Die **ModificationFunctionMapping** -Element der mapping-Spezifikationssprache (MSL) ordnet das Einfügen, aktualisieren und Löschfunktionen eines Entitätstyps zu gespeicherten Prozeduren in der zugrunde liegenden Datenbank konzeptionellen Modell.</span><span class="sxs-lookup"><span data-stu-id="964b7-563">The **ModificationFunctionMapping** element in mapping specification language (MSL) maps the insert, update, and delete functions of a conceptual model entity type to stored procedures in the underlying database.</span></span> <span data-ttu-id="964b7-564">Die **ModificationFunctionMapping** Element auch ordnen Sie die Insert und delete-Funktionen für m: n Zuordnungen im konzeptionellen Modell gespeicherten Prozeduren in der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="964b7-564">The **ModificationFunctionMapping** element can also map the insert and delete functions for many-to-many associations in the conceptual model to stored procedures in the underlying database.</span></span> <span data-ttu-id="964b7-565">Gespeicherte Prozeduren, denen Änderungsfunktionen zugeordnet werden, müssen im Speichermodell deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-565">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="964b7-566">Weitere Informationen finden Sie in der Function-Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="964b7-566">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="964b7-567">Wenn Sie nicht zuordnen alle drei Einfüge-, update oder delete-Vorgänge eines Entitätstyps zu gespeicherten Prozeduren, wird die nicht zugeordneten Vorgänge fehl, wenn zur Laufzeit ausgeführt und ein "UpdateException" ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="964b7-567">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>


> [!NOTE]
> <span data-ttu-id="964b7-568">Wenn die Änderungsfunktionen für eine Entität in einer Vererbungshierarchie gespeicherten Prozeduren zugeordnet werden, dann müssen die Änderungsfunktionen für alle Typen in der Hierarchie gespeicherten Prozeduren zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-568">If the modification functions for one entity in an inheritance hierarchy are mapped to stored procedures, then modification functions for all types in the hierarchy must be mapped to stored procedures.</span></span>

<span data-ttu-id="964b7-569">Die **ModificationFunctionMapping** Element kann ein untergeordnetes Element des EntityTypeMapping-Element oder das AssociationSetMapping-Element sein.</span><span class="sxs-lookup"><span data-stu-id="964b7-569">The **ModificationFunctionMapping** element can be a child of the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

<span data-ttu-id="964b7-570">Die **ModificationFunctionMapping** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="964b7-570">The **ModificationFunctionMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="964b7-571">DeleteFunction (0 (null) oder 1)</span><span class="sxs-lookup"><span data-stu-id="964b7-571">DeleteFunction (zero or one)</span></span>
-   <span data-ttu-id="964b7-572">InsertFunction (0 (null) oder 1)</span><span class="sxs-lookup"><span data-stu-id="964b7-572">InsertFunction (zero or one)</span></span>
-   <span data-ttu-id="964b7-573">UpdateFunction (0 (null) oder 1)</span><span class="sxs-lookup"><span data-stu-id="964b7-573">UpdateFunction (zero or one)</span></span>

<span data-ttu-id="964b7-574">Es sind keine Attribute für die **ModificationFunctionMapping** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-574">No attributes are applicable to the **ModificationFunctionMapping** element.</span></span>

### <a name="example"></a><span data-ttu-id="964b7-575">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-575">Example</span></span>

<span data-ttu-id="964b7-576">Das folgende Beispiel zeigt die Entitätenmenge für die Zuordnung der **Personen** Entitätenmenge im Modell "School".</span><span class="sxs-lookup"><span data-stu-id="964b7-576">The following example shows the entity set mapping for the **People** entity set in the School model.</span></span> <span data-ttu-id="964b7-577">Zusätzlich zur spaltenzuordnung für die **Person** Entitätstyp, der die Zuordnung der INSERT-, aktualisieren und löschen Sie die Funktionen des die **Person** angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-577">In addition to the column mapping for the **Person** entity type, the mapping of the insert, update, and delete functions of the **Person** type are shown.</span></span> <span data-ttu-id="964b7-578">Die Funktionen, denen sie zugeordnet werden, sind im Speichermodell deklariert.</span><span class="sxs-lookup"><span data-stu-id="964b7-578">The functions that are mapped to are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="964b7-579">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-579">Example</span></span>

<span data-ttu-id="964b7-580">Im folgende Beispiel wird gezeigt, wie für die Zuordnung der **CourseInstructor** Zuordnungssatz in das Modell "School".</span><span class="sxs-lookup"><span data-stu-id="964b7-580">The following example shows the association set mapping for the **CourseInstructor** association set in the School model.</span></span> <span data-ttu-id="964b7-581">Zusätzlich zur spaltenzuordnung für die **CourseInstructor** -Zuordnung wird die Zuordnung der INSERT- und Delete-Funktionen des die **CourseInstructor** -Zuordnung gezeigt.</span><span class="sxs-lookup"><span data-stu-id="964b7-581">In addition to the column mapping for the **CourseInstructor** association, the mapping of the insert and delete functions of the **CourseInstructor** association are shown.</span></span> <span data-ttu-id="964b7-582">Die Funktionen, denen sie zugeordnet werden, sind im Speichermodell deklariert.</span><span class="sxs-lookup"><span data-stu-id="964b7-582">The functions that are mapped to are declared in the storage model.</span></span>

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
 

 

## <a name="queryview-element-msl"></a><span data-ttu-id="964b7-583">QueryView-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="964b7-583">QueryView Element (MSL)</span></span>

<span data-ttu-id="964b7-584">Die **QueryView** -Element der mapping-Spezifikationssprache (MSL) definiert eine schreibgeschützte Zuordnung eines Entitätstyp oder einer Zuordnung im konzeptionellen Modell und eine Tabelle in der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="964b7-584">The **QueryView** element in mapping specification language (MSL) defines a read-only mapping between an entity type or association in the conceptual model and a table in the underlying database.</span></span> <span data-ttu-id="964b7-585">Die Zuordnung wird definiert, mit einer Entity SQL-Abfrage, die für das Speichermodell ausgewertet wird, und das Resultset im Hinblick auf eine Entität oder eine Zuordnung im konzeptionellen Modell ausgedrückt.</span><span class="sxs-lookup"><span data-stu-id="964b7-585">The mapping is defined with an Entity SQL query that is evaluated against the storage model, and you express the result set in terms of an entity or association in the conceptual model.</span></span> <span data-ttu-id="964b7-586">Da Abfragesichten schreibgeschützt sind, können die durch Abfragesichten definierten Typen nicht mit herkömmlichen Aktualisierungsbefehlen aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-586">Because query views are read-only, you cannot use standard update commands to update types that are defined by query views.</span></span> <span data-ttu-id="964b7-587">Diese Typen können mithilfe von Änderungsfunktionen aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-587">You can make updates to these types by using modification functions.</span></span> <span data-ttu-id="964b7-588">Weitere Informationen finden Sie unter Vorgehensweise: Zuordnen von Änderungsfunktionen zu gespeicherten Prozeduren.</span><span class="sxs-lookup"><span data-stu-id="964b7-588">For more information, see How to: Map Modification Functions to Stored Procedures.</span></span>

> [!NOTE]
> <span data-ttu-id="964b7-589">In der **QueryView** -Element, Entity SQL-Ausdrücke, die enthalten **GroupBy**, gruppenaggregate und Navigationseigenschaften werden nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="964b7-589">In the **QueryView** element, Entity SQL expressions that contain **GroupBy**, group aggregates, or navigation properties are not supported.</span></span>

 

<span data-ttu-id="964b7-590">Die **QueryView** Element kann ein untergeordnetes Element des EntitySetMapping-Element oder das AssociationSetMapping-Element sein.</span><span class="sxs-lookup"><span data-stu-id="964b7-590">The **QueryView** element can be a child of the EntitySetMapping element or the AssociationSetMapping element.</span></span> <span data-ttu-id="964b7-591">Im ersten Fall definiert die Abfrageansicht eine schreibgeschützte Zuordnung für eine Entität im konzeptionellen Modell.</span><span class="sxs-lookup"><span data-stu-id="964b7-591">In the former case, the query view defines a read-only mapping for an entity in the conceptual model.</span></span> <span data-ttu-id="964b7-592">Im zweiten Fall definiert die Abfrageansicht eine schreibgeschützte Zuordnung für eine Zuordnung im konzeptionellen Modell.</span><span class="sxs-lookup"><span data-stu-id="964b7-592">In the latter case, the query view defines a read-only mapping for an association in the conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="964b7-593">Wenn die **AssociationSetMapping** Element ist für eine Zuordnung mit einer referenziellen Einschränkung, die **AssociationSetMapping** Element wird ignoriert.</span><span class="sxs-lookup"><span data-stu-id="964b7-593">If the **AssociationSetMapping** element is for an association with a referential constraint, the **AssociationSetMapping** element is ignored.</span></span> <span data-ttu-id="964b7-594">Weitere Informationen finden Sie unter ReferentialConstraint-Element (CSDL).</span><span class="sxs-lookup"><span data-stu-id="964b7-594">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="964b7-595">Die **QueryView** Element kann keine untergeordneten Elemente aufweisen.</span><span class="sxs-lookup"><span data-stu-id="964b7-595">The **QueryView** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="964b7-596">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="964b7-596">Applicable Attributes</span></span>

<span data-ttu-id="964b7-597">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **QueryView** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-597">The following table describes the attributes that can be applied to the **QueryView** element.</span></span>

| <span data-ttu-id="964b7-598">Attributname</span><span class="sxs-lookup"><span data-stu-id="964b7-598">Attribute Name</span></span> | <span data-ttu-id="964b7-599">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="964b7-599">Is Required</span></span> | <span data-ttu-id="964b7-600">Wert</span><span class="sxs-lookup"><span data-stu-id="964b7-600">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="964b7-601">**Typname**</span><span class="sxs-lookup"><span data-stu-id="964b7-601">**TypeName**</span></span>   | <span data-ttu-id="964b7-602">Nein</span><span class="sxs-lookup"><span data-stu-id="964b7-602">No</span></span>          | <span data-ttu-id="964b7-603">Der Name des konzeptionellen Modelltyps, der durch die Abfrageansicht zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-603">The name of the conceptual model type that is being mapped by the query view.</span></span> |

### <a name="example"></a><span data-ttu-id="964b7-604">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-604">Example</span></span>

<span data-ttu-id="964b7-605">Das folgende Beispiel zeigt die **QueryView** -Element als untergeordnetes Element des der **EntitySetMapping** Element und definiert eine abfrageansichtszuordnung für den **Abteilung** Entitätstyp in der Modell "School".</span><span class="sxs-lookup"><span data-stu-id="964b7-605">The following example shows the **QueryView** element as a child of the **EntitySetMapping** element and defines a query view mapping for the **Department** entity type in the School Model.</span></span>

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

<span data-ttu-id="964b7-606">Da die Abfrage nur eine Teilmenge der Elemente zurückgibt. die **Abteilung** Typ im Speichermodell, die **Abteilung** Typ im Modell "School" basierend auf dieser Zuordnung wie folgt geändert wurde:</span><span class="sxs-lookup"><span data-stu-id="964b7-606">Because the query only returns a subset of the members of the **Department** type in the storage model, the **Department** type in the School model has been modified based on this mapping as follows:</span></span>

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

### <a name="example"></a><span data-ttu-id="964b7-607">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-607">Example</span></span>

<span data-ttu-id="964b7-608">Das folgende Beispiel zeigt die **QueryView** Element als untergeordnetes Element eine **AssociationSetMapping** Element und definiert eine schreibgeschützte Zuordnung für die `FK_Course_Department` -Zuordnung im Modell "School".</span><span class="sxs-lookup"><span data-stu-id="964b7-608">The next example shows the **QueryView** element as the child of an **AssociationSetMapping** element and defines a read-only mapping for the `FK_Course_Department` association in the School model.</span></span>

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
 
### <a name="comments"></a><span data-ttu-id="964b7-609">Kommentare</span><span class="sxs-lookup"><span data-stu-id="964b7-609">Comments</span></span>

<span data-ttu-id="964b7-610">Sie können Abfragesichten definieren, um die folgenden Szenarien zu ermöglichen:</span><span class="sxs-lookup"><span data-stu-id="964b7-610">You can define query views to enable the following scenarios:</span></span>

-   <span data-ttu-id="964b7-611">Definieren einer Entität im konzeptionellen Modell, die nicht alle Eigenschaften der Entität im Speichermodell einschließt.</span><span class="sxs-lookup"><span data-stu-id="964b7-611">Define an entity in the conceptual model that doesn't include all the properties of the entity in the storage model.</span></span> <span data-ttu-id="964b7-612">Dazu gehören Eigenschaften, die keine Standardwerte und unterstützen keine **null** Werte.</span><span class="sxs-lookup"><span data-stu-id="964b7-612">This includes properties that do not have default values and do not support **null** values.</span></span>
-   <span data-ttu-id="964b7-613">Zuordnen von berechneten Spalten im Speichermodell zu Eigenschaften von Entitätstypen im konzeptionellen Modell.</span><span class="sxs-lookup"><span data-stu-id="964b7-613">Map computed columns in the storage model to properties of entity types in the conceptual model.</span></span>
-   <span data-ttu-id="964b7-614">Definieren eines Mappings, bei dem die Bedingungen, die für die Partitionierung von Entitäten im konzeptionellen Modell verwendet werden, nicht auf Gleichheit basieren.</span><span class="sxs-lookup"><span data-stu-id="964b7-614">Define a mapping where conditions used to partition entities in the conceptual model are not based on equality.</span></span> <span data-ttu-id="964b7-615">Wenn Sie angeben, ein bedingtes Mapping mithilfe der **Bedingung** Element die angegebene Bedingung muss gleich dem angegebenen Wert.</span><span class="sxs-lookup"><span data-stu-id="964b7-615">When you specify a conditional mapping using the **Condition** element, the supplied condition must equal the specified value.</span></span> <span data-ttu-id="964b7-616">Weitere Informationen finden Sie in der Condition-Element (MSL).</span><span class="sxs-lookup"><span data-stu-id="964b7-616">For more information, see Condition Element (MSL).</span></span>
-   <span data-ttu-id="964b7-617">Zuordnen derselben Spalte im Speichermodell zu mehreren Typen im konzeptionellen Modell.</span><span class="sxs-lookup"><span data-stu-id="964b7-617">Map the same column in the storage model to multiple types in the conceptual model.</span></span>
-   <span data-ttu-id="964b7-618">Zuordnen mehrerer Typen zu derselben Tabelle.</span><span class="sxs-lookup"><span data-stu-id="964b7-618">Map multiple types to the same table.</span></span>
-   <span data-ttu-id="964b7-619">Definieren von Zuordnungen im konzeptionellen Modell, die nicht auf Fremdschlüsseln im relationalen Schema basieren.</span><span class="sxs-lookup"><span data-stu-id="964b7-619">Define associations in the conceptual model that are not based on foreign keys in the relational schema.</span></span>
-   <span data-ttu-id="964b7-620">Verwenden benutzerdefinierter Geschäftslogik, um die Werte von Eigenschaften im konzeptionellen Modell festzulegen.</span><span class="sxs-lookup"><span data-stu-id="964b7-620">Use custom business logic to set the value of properties in the conceptual model.</span></span> <span data-ttu-id="964b7-621">Beispielsweise könnten Sie den Zeichenfolgenwert "T" in der Datenquelle auf einen Wert des zuordnen **"true"**, ein boolescher Wert, die im konzeptionellen Modell.</span><span class="sxs-lookup"><span data-stu-id="964b7-621">For example, you could map the string value "T" in the data source to a value of **true**, a Boolean, in the conceptual model.</span></span>
-   <span data-ttu-id="964b7-622">Definieren bedingter Filter für Abfrageergebnisse.</span><span class="sxs-lookup"><span data-stu-id="964b7-622">Define conditional filters for query results.</span></span>
-   <span data-ttu-id="964b7-623">Erzwingen von geringeren Dateneinschränkungen im konzeptionellen Modell als im Speichermodell.</span><span class="sxs-lookup"><span data-stu-id="964b7-623">Enforce fewer restrictions on data in the conceptual model than in the storage model.</span></span> <span data-ttu-id="964b7-624">Angenommen, Sie konnten, eine Eigenschaft im konzeptionellen Modell NULL-Werte zulässt, selbst wenn die Spalte, der sie zugeordnet ist, nicht unterstützt **null**Werte.</span><span class="sxs-lookup"><span data-stu-id="964b7-624">For example, you could make a property in the conceptual model nullable even if the column to which it is mapped does not support **null**values.</span></span>

<span data-ttu-id="964b7-625">Die folgenden Aspekte gelten, wenn Sie Abfragesichten für Entitäten definieren:</span><span class="sxs-lookup"><span data-stu-id="964b7-625">The following considerations apply when you define query views for entities:</span></span>

-   <span data-ttu-id="964b7-626">Abfragesichten sind schreibgeschützt.</span><span class="sxs-lookup"><span data-stu-id="964b7-626">Query views are read-only.</span></span> <span data-ttu-id="964b7-627">Sie können Entitäten nur mit Änderungsfunktionen aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="964b7-627">You can only make updates to entities by using modification functions.</span></span>
-   <span data-ttu-id="964b7-628">Wenn Sie eine Entität durch eine Abfragesicht definieren, müssen Sie auch alle verknüpften Entitäten durch Abfragesichten definieren.</span><span class="sxs-lookup"><span data-stu-id="964b7-628">When you define an entity type by a query view, you must also define all related entities by query views.</span></span>
-   <span data-ttu-id="964b7-629">Wenn Sie eine m: n Zuordnung zu einer Entität im Speichermodell, die eine Linktabelle im relationalen Schema darstellt zuordnen, müssen Sie definieren eine **QueryView** Element in der **AssociationSetMapping** -Element für diese Linktabelle.</span><span class="sxs-lookup"><span data-stu-id="964b7-629">When you map a many-to-many association to an entity in the storage model that represents a link table in the relational schema, you must define a **QueryView** element in the **AssociationSetMapping** element for this link table.</span></span>
-   <span data-ttu-id="964b7-630">Abfragesichten müssen für alle Typen in einer Typhierarchie definiert werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-630">Query views must be defined for all types in a type hierarchy.</span></span> <span data-ttu-id="964b7-631">Dazu stehen Ihnen folgende Möglichkeiten zur Verfügung:</span><span class="sxs-lookup"><span data-stu-id="964b7-631">You can do this in the following ways:</span></span>
-   -   <span data-ttu-id="964b7-632">Mit einem einzelnen **QueryView** -Element, das eine Entity SQL-Abfrage gibt an, die eine Union aller Entitätstypen in der Hierarchie zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="964b7-632">With a single **QueryView** element that specifies a single Entity SQL query that returns a union of all of the entity types in the hierarchy.</span></span>
    -   <span data-ttu-id="964b7-633">Mit einem einzelnen **QueryView** -Element, das eine Entity SQL-Abfrage gibt an, die die Groß-/KLEINSCHREIBUNG-Operator verwendet, um einen bestimmten Typ in der Hierarchie zurückzugeben auf Grundlage einer bestimmten Bedingung.</span><span class="sxs-lookup"><span data-stu-id="964b7-633">With a single **QueryView** element that specifies a single Entity SQL query that uses the CASE operator to return a specific entity type in the hierarchy based on a specific condition.</span></span>
    -   <span data-ttu-id="964b7-634">Mit einem zusätzlichen **QueryView** -Element für einen bestimmten Typ in der Hierarchie.</span><span class="sxs-lookup"><span data-stu-id="964b7-634">With an additional **QueryView** element for a specific type in the hierarchy.</span></span> <span data-ttu-id="964b7-635">In diesem Fall verwenden Sie die **TypeName** Attribut der **QueryView** Element, um den Entitätstyp für jede Sicht anzugeben.</span><span class="sxs-lookup"><span data-stu-id="964b7-635">In this case, use the **TypeName** attribute of the **QueryView** element to specify the entity type for each view.</span></span>
-   <span data-ttu-id="964b7-636">Wenn eine Abfragesicht definiert ist, können Sie nicht angeben der **StorageSetName** -Attribut für die **EntitySetMapping** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-636">When a query view is defined, you cannot specify the **StorageSetName** attribute on the **EntitySetMapping** element.</span></span>
-   <span data-ttu-id="964b7-637">Wenn eine Abfragesicht definiert ist, die **EntitySetMapping**Element kann nicht gleichzeitig enthalten **Eigenschaft** Zuordnungen.</span><span class="sxs-lookup"><span data-stu-id="964b7-637">When a query view is defined, the **EntitySetMapping**element cannot also contain **Property** mappings.</span></span>

## <a name="resultbinding-element-msl"></a><span data-ttu-id="964b7-638">ResultBinding-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="964b7-638">ResultBinding Element (MSL)</span></span>

<span data-ttu-id="964b7-639">Die **ResultBinding** -Element der mapping-Spezifikationssprache (MSL) ordnet SpalteWerte, die zurückgegeben werden von gespeicherten Prozeduren, Entitätseigenschaften im konzeptionellen Modell beim Entitätstyp ändernden Funktionen zugeordnet werden an die gespeicherte die Prozeduren in der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="964b7-639">The **ResultBinding** element in mapping specification language (MSL) maps column values that are returned by stored procedures to entity properties in the conceptual model when entity type modification functions are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="964b7-640">Gespeicherte Prozedur z. B. wenn der Wert der Identitätsspalte zurückgegeben wird, von einer Insert die **ResultBinding** Element wird den zurückgegebenen Wert einer Entitätstypeigenschaft im konzeptionellen Modell zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="964b7-640">For example, when the value of an identity column is returned by an insert stored procedure, the **ResultBinding** element maps the returned value to an entity type property in the conceptual model.</span></span>

<span data-ttu-id="964b7-641">Die **ResultBinding** Element kann ein untergeordnetes Element der InsertFunction-Element oder die UpdateFunction-Element sein.</span><span class="sxs-lookup"><span data-stu-id="964b7-641">The **ResultBinding** element can be child of the InsertFunction element or the UpdateFunction element.</span></span>

<span data-ttu-id="964b7-642">Die **ResultBinding** Element kann keine untergeordneten Elemente aufweisen.</span><span class="sxs-lookup"><span data-stu-id="964b7-642">The **ResultBinding** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="964b7-643">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="964b7-643">Applicable Attributes</span></span>

<span data-ttu-id="964b7-644">Die folgende Tabelle beschreibt die Attribute für die **ResultBinding** Element:</span><span class="sxs-lookup"><span data-stu-id="964b7-644">The following table describes the attributes that are applicable to the **ResultBinding** element:</span></span>

| <span data-ttu-id="964b7-645">Attributname</span><span class="sxs-lookup"><span data-stu-id="964b7-645">Attribute Name</span></span> | <span data-ttu-id="964b7-646">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="964b7-646">Is Required</span></span> | <span data-ttu-id="964b7-647">Wert</span><span class="sxs-lookup"><span data-stu-id="964b7-647">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="964b7-648">**Name**</span><span class="sxs-lookup"><span data-stu-id="964b7-648">**Name**</span></span>       | <span data-ttu-id="964b7-649">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-649">Yes</span></span>         | <span data-ttu-id="964b7-650">Der Name der Entitätseigenschaft im konzeptionellen Modell, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-650">The name of the entity property in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="964b7-651">**Spaltenname**</span><span class="sxs-lookup"><span data-stu-id="964b7-651">**ColumnName**</span></span> | <span data-ttu-id="964b7-652">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-652">Yes</span></span>         | <span data-ttu-id="964b7-653">Der Name der Spalte, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-653">The name of the column being mapped.</span></span>                                          |

### <a name="example"></a><span data-ttu-id="964b7-654">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-654">Example</span></span>

<span data-ttu-id="964b7-655">Das folgende Beispiel basiert auf dem Modell "School" und zeigt eine **InsertFunction** Element verwendet, um die Einfügefunktion zuzuordnen der **Person** Entitätstyp, der die **InsertPerson** gespeicherte Prozedur.</span><span class="sxs-lookup"><span data-stu-id="964b7-655">The following example is based on the School model and shows an **InsertFunction** element used to map the insert function of the **Person** entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="964b7-656">(Die **InsertPerson** gespeicherte Prozedur wird im folgenden dargestellt und wird im Speichermodell deklariert.) Ein **ResultBinding** -Element wird verwendet, um einen Spaltenwert zuzuordnen, die von der gespeicherten Prozedur zurückgegeben wird (**NewPersonID**), einer Entitätstypeigenschaft (**PersonID**).</span><span class="sxs-lookup"><span data-stu-id="964b7-656">(The **InsertPerson** stored procedure is shown below and is declared in the storage model.) A **ResultBinding** element is used to map a column value that is returned by the stored procedure (**NewPersonID**) to an entity type property (**PersonID**).</span></span>

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

<span data-ttu-id="964b7-657">Die folgende Transact-SQL wird beschrieben, die **InsertPerson** gespeicherte Prozedur:</span><span class="sxs-lookup"><span data-stu-id="964b7-657">The following Transact-SQL describes the **InsertPerson** stored procedure:</span></span>

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

## <a name="resultmapping-element-msl"></a><span data-ttu-id="964b7-658">ResultMapping-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="964b7-658">ResultMapping Element (MSL)</span></span>

<span data-ttu-id="964b7-659">Die **ResultMapping** -Element der mapping-Spezifikationssprache (MSL) definiert die Zuordnung zwischen einem Funktionsimport im konzeptionellen Modell und eine gespeicherte Prozedur in der zugrunde liegenden Datenbank, wenn Folgendes zutrifft:</span><span class="sxs-lookup"><span data-stu-id="964b7-659">The **ResultMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="964b7-660">Der Funktionsimport gibt einen Entitätstyp des konzeptionellen Modells oder einen komplexen Typ zurück.</span><span class="sxs-lookup"><span data-stu-id="964b7-660">The function import returns a conceptual model entity type or complex type.</span></span>
-   <span data-ttu-id="964b7-661">Die Namen der Spalten, die von der gespeicherten Prozedur zurückgegeben werden, entsprechen nicht exakt den Namen der Eigenschaften für den Entitätstyp oder den komplexem Typ.</span><span class="sxs-lookup"><span data-stu-id="964b7-661">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the entity type or complex type.</span></span>

<span data-ttu-id="964b7-662">Standardmäßig basiert die Zuordnung der von einer gespeicherten Prozedur zurückgegebenen Spalten zu einem Entitätstyp oder einem komplexen Typ auf den Spalten- und Eigenschaftennamen.</span><span class="sxs-lookup"><span data-stu-id="964b7-662">By default, the mapping between the columns returned by a stored procedure and an entity type or complex type is based on column and property names.</span></span> <span data-ttu-id="964b7-663">Wenn Spaltennamen nicht exakt Eigenschaftennamen übereinstimmen, müssen Sie verwenden die **ResultMapping** Element, um die Zuordnung zu definieren.</span><span class="sxs-lookup"><span data-stu-id="964b7-663">If column names do not exactly match property names, you must use the **ResultMapping** element to define the mapping.</span></span> <span data-ttu-id="964b7-664">Ein Beispiel für die standardzuordnung finden Sie unter FunctionImportMapping-Element (MSL).</span><span class="sxs-lookup"><span data-stu-id="964b7-664">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="964b7-665">Die **ResultMapping** Element ist ein untergeordnetes Element des FunctionImportMapping-Elements.</span><span class="sxs-lookup"><span data-stu-id="964b7-665">The **ResultMapping** element is a child element of the FunctionImportMapping element.</span></span>

<span data-ttu-id="964b7-666">Die **ResultMapping** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="964b7-666">The **ResultMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="964b7-667">EntityTypeMapping (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-667">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="964b7-668">ComplexTypeMapping</span><span class="sxs-lookup"><span data-stu-id="964b7-668">ComplexTypeMapping</span></span>

<span data-ttu-id="964b7-669">Es sind keine Attribute für die **ResultMapping** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-669">No attributes are applicable to the **ResultMapping** Element.</span></span>

### <a name="example"></a><span data-ttu-id="964b7-670">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-670">Example</span></span>

<span data-ttu-id="964b7-671">Betrachten Sie die folgende gespeicherte Prozedur:</span><span class="sxs-lookup"><span data-stu-id="964b7-671">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="964b7-672">Betrachten Sie auch den folgenden Entitätstyp des konzeptionellen Modells:</span><span class="sxs-lookup"><span data-stu-id="964b7-672">Also consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="964b7-673">Um einen Funktionsimport zu erstellen, die Instanzen des vorherigen Entitätstyps zurückgibt, die Zuordnung zwischen den Spalten, die von der gespeicherten Prozedur zurückgegeben, und der Entitätstyp muss definiert werden, einem **ResultMapping** Element:</span><span class="sxs-lookup"><span data-stu-id="964b7-673">In order to create a function import that returns instances of the previous entity type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ResultMapping** element:</span></span>

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

## <a name="scalarproperty-element-msl"></a><span data-ttu-id="964b7-674">ScalarProperty-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="964b7-674">ScalarProperty Element (MSL)</span></span>

<span data-ttu-id="964b7-675">Die **ScalarProperty** -Element der mapping-Spezifikationssprache (MSL) ordnet eine Eigenschaft auf einen Entitätstyp des konzeptionellen Modells, den komplexen Typ oder die Zuordnung einer Tabellenspalte oder der Parameter der gespeicherten Prozedur in der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="964b7-675">The **ScalarProperty** element in mapping specification language (MSL) maps a property on a conceptual model entity type, complex type, or association to a table column or stored procedure parameter in the underlying database.</span></span>

> [!NOTE]
> <span data-ttu-id="964b7-676">Gespeicherte Prozeduren, denen Änderungsfunktionen zugeordnet werden, müssen im Speichermodell deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-676">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="964b7-677">Weitere Informationen finden Sie in der Function-Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="964b7-677">For more information, see Function Element (SSDL).</span></span>

<span data-ttu-id="964b7-678">Die **ScalarProperty** Element kann ein untergeordnetes Element der folgenden Elemente sein:</span><span class="sxs-lookup"><span data-stu-id="964b7-678">The **ScalarProperty** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="964b7-679">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="964b7-679">MappingFragment</span></span>
-   <span data-ttu-id="964b7-680">InsertFunction</span><span class="sxs-lookup"><span data-stu-id="964b7-680">InsertFunction</span></span>
-   <span data-ttu-id="964b7-681">UpdateFunction</span><span class="sxs-lookup"><span data-stu-id="964b7-681">UpdateFunction</span></span>
-   <span data-ttu-id="964b7-682">DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="964b7-682">DeleteFunction</span></span>
-   <span data-ttu-id="964b7-683">EndProperty</span><span class="sxs-lookup"><span data-stu-id="964b7-683">EndProperty</span></span>
-   <span data-ttu-id="964b7-684">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="964b7-684">ComplexProperty</span></span>
-   <span data-ttu-id="964b7-685">ResultMapping</span><span class="sxs-lookup"><span data-stu-id="964b7-685">ResultMapping</span></span>

<span data-ttu-id="964b7-686">Als untergeordnetes Element der **MappingFragment**, **ComplexProperty**, oder **EndProperty** -Element, das **ScalarProperty** -Element ordnet eine Eigenschaft im konzeptionellen Modell an eine Spalte in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="964b7-686">As a child of the **MappingFragment**, **ComplexProperty**, or **EndProperty** element, the **ScalarProperty** element maps a property in the conceptual model to a column in the database.</span></span> <span data-ttu-id="964b7-687">Als untergeordnetes Element der **InsertFunction**, **UpdateFunction**, oder **DeleteFunction** -Element, das **ScalarProperty** -Element ordnet eine Eigenschaft im konzeptionellen Modell Parameter einer gespeicherten Prozedur.</span><span class="sxs-lookup"><span data-stu-id="964b7-687">As a child of the **InsertFunction**, **UpdateFunction**, or **DeleteFunction** element, the **ScalarProperty** element maps a property in the conceptual model to a stored procedure parameter.</span></span>

<span data-ttu-id="964b7-688">Die **ScalarProperty** Element kann keine untergeordneten Elemente aufweisen.</span><span class="sxs-lookup"><span data-stu-id="964b7-688">The **ScalarProperty** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="964b7-689">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="964b7-689">Applicable Attributes</span></span>

<span data-ttu-id="964b7-690">Die Attribute, die zum Anwenden der **ScalarProperty** Element unterscheiden sich abhängig von der Rolle des Elements.</span><span class="sxs-lookup"><span data-stu-id="964b7-690">The attributes that apply to the **ScalarProperty** element differ depending on the role of the element.</span></span>

<span data-ttu-id="964b7-691">Die folgende Tabelle beschreibt die Attribute, die gelten, dass bei der **ScalarProperty** Element wird verwendet, um eine Eigenschaft des konzeptionellen Modells einer Spalte in der Datenbank zuzuordnen:</span><span class="sxs-lookup"><span data-stu-id="964b7-691">The following table describes the attributes that are applicable when the **ScalarProperty** element is used to map a conceptual model property to a column in the database:</span></span>

| <span data-ttu-id="964b7-692">Attributname</span><span class="sxs-lookup"><span data-stu-id="964b7-692">Attribute Name</span></span> | <span data-ttu-id="964b7-693">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="964b7-693">Is Required</span></span> | <span data-ttu-id="964b7-694">Wert</span><span class="sxs-lookup"><span data-stu-id="964b7-694">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="964b7-695">**Name**</span><span class="sxs-lookup"><span data-stu-id="964b7-695">**Name**</span></span>       | <span data-ttu-id="964b7-696">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-696">Yes</span></span>         | <span data-ttu-id="964b7-697">Der Name der Eigenschaft im konzeptionellen Modell, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-697">The name of the conceptual model property that is being mapped.</span></span> |
| <span data-ttu-id="964b7-698">**Spaltenname**</span><span class="sxs-lookup"><span data-stu-id="964b7-698">**ColumnName**</span></span> | <span data-ttu-id="964b7-699">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-699">Yes</span></span>         | <span data-ttu-id="964b7-700">Der Name der Tabellenspalte, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-700">The name of the table column that is being mapped.</span></span>              |

<span data-ttu-id="964b7-701">Die folgende Tabelle beschreibt die Attribute für die **ScalarProperty** -Element, wenn es verwendet wird, um Parameter einer gespeicherten Prozedur eine Eigenschaft im konzeptionellen Modell zuzuordnen:</span><span class="sxs-lookup"><span data-stu-id="964b7-701">The following table describes the attributes that are applicable to the **ScalarProperty** element when it is used to map a conceptual model property to a stored procedure parameter:</span></span>

| <span data-ttu-id="964b7-702">Attributname</span><span class="sxs-lookup"><span data-stu-id="964b7-702">Attribute Name</span></span>    | <span data-ttu-id="964b7-703">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="964b7-703">Is Required</span></span> | <span data-ttu-id="964b7-704">Wert</span><span class="sxs-lookup"><span data-stu-id="964b7-704">Value</span></span>                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="964b7-705">**Name**</span><span class="sxs-lookup"><span data-stu-id="964b7-705">**Name**</span></span>          | <span data-ttu-id="964b7-706">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-706">Yes</span></span>         | <span data-ttu-id="964b7-707">Der Name der Eigenschaft im konzeptionellen Modell, die zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-707">The name of the conceptual model property that is being mapped.</span></span>                                                                                 |
| <span data-ttu-id="964b7-708">**ParameterName**</span><span class="sxs-lookup"><span data-stu-id="964b7-708">**ParameterName**</span></span> | <span data-ttu-id="964b7-709">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-709">Yes</span></span>         | <span data-ttu-id="964b7-710">Der Name des Parameters, der zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-710">The name of the parameter that is being mapped.</span></span>                                                                                                 |
| <span data-ttu-id="964b7-711">**Version**</span><span class="sxs-lookup"><span data-stu-id="964b7-711">**Version**</span></span>       | <span data-ttu-id="964b7-712">Nein</span><span class="sxs-lookup"><span data-stu-id="964b7-712">No</span></span>          | <span data-ttu-id="964b7-713">**Aktuelle** oder **ursprünglichen** abhängig davon, ob der aktuelle Wert oder den ursprünglichen Wert der Eigenschaft für parallelitätsüberprüfungen verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="964b7-713">**Current** or **Original** depending on whether the current value or the original value of the property should be used for concurrency checks.</span></span> |

### <a name="example"></a><span data-ttu-id="964b7-714">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-714">Example</span></span>

<span data-ttu-id="964b7-715">Das folgende Beispiel zeigt die **ScalarProperty** Element, das verwendet wird, gibt es zwei Möglichkeiten:</span><span class="sxs-lookup"><span data-stu-id="964b7-715">The following example shows the **ScalarProperty** element used in two ways:</span></span>

-   <span data-ttu-id="964b7-716">Zum Zuordnen von den Eigenschaften von den **Person** Entitätstyp, der die Spalten der **Person**Tabelle.</span><span class="sxs-lookup"><span data-stu-id="964b7-716">To map the properties of the **Person** entity type to the columns of the **Person**table.</span></span>
-   <span data-ttu-id="964b7-717">Zum Zuordnen von den Eigenschaften von der **Person** Entitätstyp, der die Parameter der **UpdatePerson** gespeicherte Prozedur.</span><span class="sxs-lookup"><span data-stu-id="964b7-717">To map the properties of the **Person** entity type to the parameters of the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="964b7-718">Die gespeicherten Prozeduren werden im Speichermodell deklariert.</span><span class="sxs-lookup"><span data-stu-id="964b7-718">The stored procedures are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="964b7-719">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-719">Example</span></span>

<span data-ttu-id="964b7-720">Das folgende Beispiel zeigt die **ScalarProperty** , das zum Zuordnen von Insert und delete-Funktionen, die von einer konzeptionellen Modellzuordnung zu gespeicherten Prozeduren in der Datenbank verwendet.</span><span class="sxs-lookup"><span data-stu-id="964b7-720">The next example shows the **ScalarProperty** element used to map the insert and delete functions of a conceptual model association to stored procedures in the database.</span></span> <span data-ttu-id="964b7-721">Die gespeicherten Prozeduren werden im Speichermodell deklariert.</span><span class="sxs-lookup"><span data-stu-id="964b7-721">The stored procedures are declared in the storage model.</span></span>

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

## <a name="updatefunction-element-msl"></a><span data-ttu-id="964b7-722">UpdateFunction-Element (MSL)</span><span class="sxs-lookup"><span data-stu-id="964b7-722">UpdateFunction Element (MSL)</span></span>

<span data-ttu-id="964b7-723">Die **UpdateFunction** -Element der mapping-Spezifikationssprache (MSL) ordnet Aktualisierungs-und Löschfunktionen eines Entitätstyps im konzeptionellen Modell einer gespeicherten Prozedur in der zugrunde liegenden Datenbank.</span><span class="sxs-lookup"><span data-stu-id="964b7-723">The **UpdateFunction** element in mapping specification language (MSL) maps the update function of an entity type in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="964b7-724">Gespeicherte Prozeduren, denen Änderungsfunktionen zugeordnet werden, müssen im Speichermodell deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-724">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="964b7-725">Weitere Informationen finden Sie in der Function-Element (SSDL).</span><span class="sxs-lookup"><span data-stu-id="964b7-725">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
>  <span data-ttu-id="964b7-726">Wenn Sie nicht zuordnen alle drei Einfüge-, update oder delete-Vorgänge eines Entitätstyps zu gespeicherten Prozeduren, wird die nicht zugeordneten Vorgänge fehl, wenn zur Laufzeit ausgeführt und ein "UpdateException" ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="964b7-726">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="964b7-727">Die **UpdateFunction** Element kann ein untergeordnetes Element des ModificationFunctionMapping-Element und angewendet auf EntityTypeMapping-Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-727">The **UpdateFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element.</span></span>

<span data-ttu-id="964b7-728">Die **UpdateFunction** -Element kann die folgenden untergeordneten Elemente aufweisen:</span><span class="sxs-lookup"><span data-stu-id="964b7-728">The **UpdateFunction** element can have the following child elements:</span></span>

-   <span data-ttu-id="964b7-729">AssociationEnd (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-729">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="964b7-730">ComplexProperty (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-730">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="964b7-731">ResultBinding (0 (null) oder 1)</span><span class="sxs-lookup"><span data-stu-id="964b7-731">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="964b7-732">ScalarProperty (null oder mehr)</span><span class="sxs-lookup"><span data-stu-id="964b7-732">ScarlarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="964b7-733">Anwendbare Attribute</span><span class="sxs-lookup"><span data-stu-id="964b7-733">Applicable Attributes</span></span>

<span data-ttu-id="964b7-734">Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **UpdateFunction** Element.</span><span class="sxs-lookup"><span data-stu-id="964b7-734">The following table describes the attributes that can be applied to the **UpdateFunction** element.</span></span>

| <span data-ttu-id="964b7-735">Attributname</span><span class="sxs-lookup"><span data-stu-id="964b7-735">Attribute Name</span></span>            | <span data-ttu-id="964b7-736">Ist erforderlich</span><span class="sxs-lookup"><span data-stu-id="964b7-736">Is Required</span></span> | <span data-ttu-id="964b7-737">Wert</span><span class="sxs-lookup"><span data-stu-id="964b7-737">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="964b7-738">**Funktionsname**</span><span class="sxs-lookup"><span data-stu-id="964b7-738">**FunctionName**</span></span>          | <span data-ttu-id="964b7-739">Ja</span><span class="sxs-lookup"><span data-stu-id="964b7-739">Yes</span></span>         | <span data-ttu-id="964b7-740">Der mit einem Namespace qualifizierte Name der gespeicherten Prozedur, der die Aktualisierungsfunktion zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="964b7-740">The namespace-qualified name of the stored procedure to which the update function is mapped.</span></span> <span data-ttu-id="964b7-741">Die gespeicherte Prozedur muss im Speichermodell deklariert werden.</span><span class="sxs-lookup"><span data-stu-id="964b7-741">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="964b7-742">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="964b7-742">**RowsAffectedParameter**</span></span> | <span data-ttu-id="964b7-743">Nein</span><span class="sxs-lookup"><span data-stu-id="964b7-743">No</span></span>          | <span data-ttu-id="964b7-744">Der Name des Ausgabeparameters, der die Anzahl der betroffenen Zeilen zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="964b7-744">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

### <a name="example"></a><span data-ttu-id="964b7-745">Beispiel</span><span class="sxs-lookup"><span data-stu-id="964b7-745">Example</span></span>

<span data-ttu-id="964b7-746">Das folgende Beispiel basiert auf dem Modell "School" und zeigt die **UpdateFunction** Element verwendet, um die Aktualisierungsfunktion der **Person** Entitätstyp, der die **UpdatePerson** gespeicherte Prozedur.</span><span class="sxs-lookup"><span data-stu-id="964b7-746">The following example is based on the School model and shows the **UpdateFunction** element used to map update function of the **Person** entity type to the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="964b7-747">Die **UpdatePerson** wird die gespeicherte Prozedur im Speichermodell deklariert.</span><span class="sxs-lookup"><span data-stu-id="964b7-747">The **UpdatePerson** stored procedure is declared in the storage model.</span></span>

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
