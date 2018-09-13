---
title: MSL-Spezifikation - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 9519155422d8542d4a14bc1c612e91ebc22bf15e
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490557"
---
# <a name="msl-specification"></a>MSL-Spezifikation
Mapping-Spezifikationssprache (MSL) wird eine XML-basierte Sprache, die die Zuordnung zwischen dem konzeptionellen Modell und Speichermodell einer Entity Framework-Anwendung beschreibt ist.

In einer Entity Framework-Anwendung wird die Mapping-Metadaten aus einer MSL-Datei (geschrieben in MSL) zur Buildzeit geladen. Entitätsframework verwendet Mapping-Metadaten zur Laufzeit verwendet, um Abfragen für das konzeptionelle Modell in datenspeicherspezifische Befehle zu übersetzen.

Die Entity Framework Designer (EF-Designer) speichert Informationen über die Zuordnung in einer EDMX-Datei zur Entwurfszeit. Zur Erstellungszeit verwendet der Entity Designer Informationen in einer EDMX-Datei die MSL-Datei zu erstellen, die zur Laufzeit vom Entity Framework benötigt wird

Namen aller konzeptionellen oder Speichermodelltypen, auf denen in MSL verwiesen wird, müssen mit dem jeweiligen Namespacenamen qualifiziert werden. Weitere Informationen zu den Namespacenamen des konzeptionellen Modells, finden Sie unter [CSDL-Spezifikation](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md). Weitere Informationen zu den Namespacenamen des Storage-Modell, finden Sie unter [SSDL-Spezifikation](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

MSL-Versionen unterscheiden sich von XML-Namespaces.

| MSL-Version | XML-Namespace                                        |
|:------------|:-----------------------------------------------------|
| MSL v1      | Urn: Schemas-Microsoft-Com:windows:storage:mapping:CS |
| MSL-v2      | http://schemas.microsoft.com/ado/2008/09/mapping/cs  |
| MSL v3      | http://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a>Alias-Element (MSL)

Die **Alias** -Element der mapping-Spezifikationssprache (MSL) ist ein untergeordnetes Element des das Mapping-Element, das verwendet wird, um Aliase für konzeptionelle Modell und speichermodellnamespaces zu definieren. Namen aller konzeptionellen oder Speichermodelltypen, auf denen in MSL verwiesen wird, müssen mit dem jeweiligen Namespacenamen qualifiziert werden. Informationen zu den Namespacenamen des konzeptionellen Modells finden Sie in der Schema-Element (CSDL). Informationen zu den Namespacenamen des Storage-Modell finden Sie in der Schema-Element (SSDL).

Die **Alias** Element darf keine untergeordneten Elemente.

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Alias** Element.

| Attributname | Ist erforderlich | Wert                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| **Key**        | Ja         | Der Alias für den Namespace, der angegeben wird die **Wert** Attribut. |
| **Wert**      | Ja         | Der Namespace für die der Wert des der **Schlüssel** Element ist ein Alias.     |

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **Alias** Element, das einen Alias definiert `c`, für die Typen, die im konzeptionellen Modell definiert sind.

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

## <a name="associationend-element-msl"></a>AssociationEnd-Element (MSL)

Die **AssociationEnd** -Element der mapping-Spezifikationssprache (MSL) wird verwendet, wenn die Änderungsfunktionen eines Entitätstyps im konzeptionellen Modell gespeicherten Prozeduren in der zugrunde liegenden Datenbank zugeordnet werden. Wenn eine Änderung, die gespeicherte Prozedur einen Parameter übernimmt, dessen Wert wird in einer Zuordnungseigenschaft aufrechterhalten, der **AssociationEnd** Element wird der Parameter den Wert der Eigenschaft zugeordnet. Weitere Informationen finden Sie im untenstehenden Beispiel.

Weitere Informationen zum Zuordnen von Änderungsfunktionen von Entitätstypen zu gespeicherten Prozeduren finden Sie unter ModificationFunctionMapping-Element (MSL) und die exemplarische Vorgehensweise: Zuordnen einer Entität zu gespeicherten Prozeduren.

Die **AssociationEnd** -Element kann die folgenden untergeordneten Elemente aufweisen:

-   ScalarProperty

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute für die **AssociationEnd** Element.

| Attributname     | Ist erforderlich | Wert                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **AssociationSet** | Ja         | Der Name der Zuordnung, die zugeordnet wird.                                                                                                                                 |
| **From**           | Ja         | Der Wert des der **FromRole** Attribut der Navigationseigenschaft, die der zugeordneten Zuordnung entspricht. Weitere Informationen finden Sie unter NavigationProperty-Element (CSDL). |
| **Aktion**             | Ja         | Der Wert des der **ToRole** Attribut der Navigationseigenschaft, die der zugeordneten Zuordnung entspricht. Weitere Informationen finden Sie unter NavigationProperty-Element (CSDL).   |

### <a name="example"></a>Beispiel

Betrachten Sie den folgenden Entitätstyp des konzeptionellen Modells:

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

Betrachten Sie auch die folgende gespeicherte Prozedur:

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

Um die Aktualisierungsfunktion der `Course` Entität dieser gespeicherten Prozedur müssen Sie einen Wert angeben der **"DepartmentID"** Parameter. Der Wert für `DepartmentID` entspricht keiner Eigenschaft des Entitätstyps; er ist vielmehr in einer unabhängigen Zuordnung enthalten, deren Zuordnung hier angezeigt wird:

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

Der folgende code zeigt die **AssociationEnd** Element für die Zuordnung der **"DepartmentID"** Eigenschaft der **FK\_Kurs\_Abteilung** Zuordnung, die die **UpdateCourse** gespeicherten Prozedur (zu dem die Aktualisierungsfunktion des der **Kurs** Entitätstyp zugeordnet ist):

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

## <a name="associationsetmapping-element-msl"></a>AssociationSetMapping-Element (MSL)

Die **AssociationSetMapping** -Element der mapping-Spezifikationssprache (MSL) definiert die Zuordnung zwischen einer Zuordnung im konzeptionellen Modell und zur Tabelle Spalten der zugrunde liegenden Datenbank.

Zuordnungen im konzeptionellen Modell sind Typen, deren Eigenschaften Primär- und Fremdschlüsselspalten in der zugrunde liegenden Datenbank darstellen. Die **AssociationSetMapping** -Element verwendet zwei EndProperty-Elemente, um die Zuordnungen zwischen zuordnungstypeigenschaften zu Spalten in der Datenbank zu definieren. Sie können Bedingungen für diese Zuordnungen mit der Condition-Element platzieren. Ordnen Sie die INSERT-, Update- und Löschfunktionen für Zuordnungen zu gespeicherten Prozeduren in der Datenbank mit dem ModificationFunctionMapping-Element. Definieren Sie schreibgeschützte Zuordnungen von Zuordnungen zu Spalten der Tabelle, indem Sie mit einer Entity SQL-Zeichenfolge in ein QueryView-Element.

> [!NOTE]
> Wenn eine referenzielle Einschränkung für eine Zuordnung im konzeptionellen Modell definiert ist, die Zuordnung muss nicht zugeordnet ist ein **AssociationSetMapping** Element. Wenn ein **AssociationSetMapping** -Element für eine Zuordnung, die eine referenzielle Einschränkung vorhanden ist, wird die Zuordnungen definiert, der **AssociationSetMapping** Element wird ignoriert. Weitere Informationen finden Sie unter ReferentialConstraint-Element (CSDL).

Die **AssociationSetMapping** -Element kann die folgenden untergeordneten Elemente aufweisen

-   QueryView (0 (null) oder 1)
-   EndProperty (0 (null) oder zwei)
-   Bedingung (null oder mehr)
-   ModificationFunctionMapping (0 (null) oder 1)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **AssociationSetMapping** Element.

| Attributname     | Ist erforderlich | Wert                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| **Name**           | Ja         | Der Name des konzeptionellen Modell-Zuordnungssatzes, der zugeordnet wird.                      |
| **Typname**       | Nein          | Der mit einem Namespace qualifizierte Name des konzeptionellen Modell-Zuordnungstyps, der zugeordnet wird. |
| **StoreEntitySet** | Nein          | Der Name der Tabelle, die zugeordnet wird.                                                 |

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **AssociationSetMapping** Element, in dem die **FK\_Kurs\_Abteilung** der ZuordnungssatzimkonzeptionellenModellzugeordnetist **Kurs** Tabelle in der Datenbank. Zuordnungen zwischen zuordnungstypeigenschaften zu Tabellenspalten werden in untergeordneten angegeben **EndProperty** Elemente.

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

## <a name="complexproperty-element-msl"></a>ComplexProperty-Element (MSL)

Ein **ComplexProperty** -Element der mapping-Spezifikationssprache (MSL) definiert die Zuordnung zwischen der Eigenschaft eines komplexen Typs in einem konzeptionellen Modell Entität den Tabellenspalten in der zugrunde liegenden Datenbank. Die Zuordnungen zu Eigenschaftenspalten sind in untergeordneten ScalarProperty-Elemente angegeben.

Die **ComplexType** Property-Element kann die folgenden untergeordneten Elemente verfügen:

-   ScalarProperty (null oder mehr)
-   **ComplexProperty** (null oder mehr)
-   ComplextTypeMapping (null oder mehr)
-   Bedingung (null oder mehr)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute für die **ComplexProperty** Element:

| Attributname | Ist erforderlich | Wert                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Name**       | Ja         | Der Name der komplexen Eigenschaft eines Entitätstyps im konzeptionellen Modell, die zugeordnet wird. |
| **Typname**   | Nein          | Der mit einem Namespace qualifizierte Name des Eigenschaftentyps im konzeptionellen Modell.                              |

### <a name="example"></a>Beispiel

Das folgende Beispiel basiert auf dem Modell "School". Dem konzeptionellen Modell wurde der folgende komplexe Typ hinzugefügt:

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

Die **"LastName"** und **FirstName** Eigenschaften der **Person** Entitätstyp wurden durch eine komplexe Eigenschaft ersetzt **Namen**:

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

Die folgende MSL-zeigt die **ComplexProperty** Element für die Zuordnung der **Namen** -Eigenschaft den Spalten in der zugrunde liegenden Datenbank:

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

## <a name="complextypemapping-element-msl"></a>ComplexTypeMapping-Element (MSL)

Die **ComplexTypeMapping** -Element der mapping-Spezifikationssprache (MSL) ist ein untergeordnetes Element von der ResultMapping-Element und definiert die Zuordnung zwischen einem Funktionsimport im konzeptionellen Modell und eine gespeicherte Prozedur in der zugrunde liegenden Datenbank, wenn Folgendes zutrifft:

-   Der Funktionsimport gibt einen konzeptionellen komplexen Typ zurück.
-   Die Namen der Spalten, die von der gespeicherten Prozedur zurückgegeben werden, entsprechen nicht genau den Namen der Eigenschaften für den komplexen Typ.

Standardmäßig basiert die Zuordnung der von einer gespeicherten Prozedur zurückgegebenen Spalten zu einem komplexen Typ auf den Spalten- und Eigenschaftennamen. Wenn Spaltennamen nicht exakt Eigenschaftennamen übereinstimmen, müssen Sie verwenden die **ComplexTypeMapping** Element, um die Zuordnung zu definieren. Ein Beispiel für die standardzuordnung finden Sie unter FunctionImportMapping-Element (MSL).

Die **ComplexTypeMapping** -Element kann die folgenden untergeordneten Elemente aufweisen:

-   ScalarProperty (null oder mehr)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute für die **ComplexTypeMapping** Element.

| Attributname | Ist erforderlich | Wert                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| **Typname**   | Ja         | Der namespacequalifizierte Name des komplexen Typs, der zugeordnet wird. |

### <a name="example"></a>Beispiel

Betrachten Sie die folgende gespeicherte Prozedur:

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

Betrachten Sie auch den folgenden komplexen Typ des konzeptionellen Modells:

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

Um einen Funktionsimport zu erstellen, die Instanzen des vorherigen komplexen Typs zurückgibt, die Zuordnung zwischen den Spalten, die von der gespeicherten Prozedur zurückgegeben, und der Entitätstyp muss definiert werden, einem **ComplexTypeMapping** Element:

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

## <a name="condition-element-msl"></a>Condition-Element (MSL)

Die **Bedingung** -Element der mapping-Spezifikationssprache (MSL) definiert Bedingungen für Zuordnungen zwischen dem konzeptionellen Modell und der zugrunde liegenden Datenbank. Die, die innerhalb eines XML-Knotens definierte Zuordnung ist gültig, wenn alle Bedingungen im angegebenen untergeordneten **Bedingung** Elemente, erfüllt werden. Andernfalls ist die Zuordnung ungültig. Angenommen, ein MappingFragment-Element enthält, eine oder mehrere **Bedingung** untergeordnete Elemente, die Zuordnung definiert, in der **MappingFragment** Knoten werden nur gültig, wenn alle der Bedingungen für die untergeordneten  **Bedingung** Elemente erfüllt werden.

Jede Bedingung kann zutreffen, entweder eine **Namen** (der Name der Entitätseigenschaft einer konzeptionellen Modell anhand des der **Namen** Attribut), oder ein **ColumnName** (der Name einer Spalte in die Datenbank, die gemäß der **ColumnName** Attribut). Wenn die **Namen** Attribut festgelegt ist, wird die Bedingung für einen Eigenschaftswert für die Entität aktiviert ist. Wenn die **ColumnName** Attribut festgelegt ist, wird die Bedingung mit einem Spaltenwert aktiviert ist. Nur eine von der **Namen** oder **ColumnName** -Attribut angegeben werden, eine **Bedingung** Element.

> [!NOTE]
> Wenn die **Bedingung** Element wird verwendet, in ein FunctionImportMapping-Element nur die **Namen** Attribut ist nicht anwendbar.

Die **Bedingung** Element kann ein untergeordnetes Element der folgenden Elemente sein:

-   AssociationSetMapping
-   ComplexProperty
-   EntitySetMapping
-   MappingFragment
-   EntityTypeMapping

Die **Bedingung** -Element kann keine untergeordneten Elemente aufweisen.

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute für die **Bedingung** Element:

| Attributname | Ist erforderlich | Wert                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Spaltenname** | Nein          | Der Name der Tabellenspalte, deren Wert zur Auswertung der Bedingung verwendet wird.                                                                                                                                                                                                                   |
| **IsNull**     | Nein          | **"True"** oder **"false"**. Wenn der Wert ist **"true"** und der Spaltenwert **null**, oder wenn der Wert ist **"false"** und der Wert der Spalte nicht **null**, die Bedingung wahr ist . Andernfalls ist die Bedingung nicht erfüllt (false). <br/> Die **IsNull** und **Wert** Attribute können nicht gleichzeitig verwendet werden. |
| **Wert**      | Nein          | Der Wert, mit dem der Spaltenwert verglichen werden soll. Wenn die Werte gleich sind, wird die Bedingung erfüllt (true). Andernfalls ist die Bedingung nicht erfüllt (false). <br/> Die **IsNull** und **Wert** Attribute können nicht gleichzeitig verwendet werden.                                                                       |
| **Name**       | Nein          | Der Name der Entitätseigenschaft im konzeptionellen Modell, deren Wert zur Auswertung der Bedingung verwendet wird. <br/> Dieses Attribut gilt nicht wenn die **Bedingung** Element innerhalb eines FunctionImportMapping-Elements verwendet wird.                                                                           |

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt **Bedingung** Elemente als untergeordnete Elemente des **MappingFragment** Elemente. Wenn **HireDate** ist nicht null und **EnrollmentDate** ist null ist, Daten zwischen zugeordnet der **SchoolModel.Instructor** Typ und die **PersonID**und **HireDate** Spalten der **Person** Tabelle. Wenn **EnrollmentDate** ist nicht null und **HireDate** ist null ist, Daten zwischen zugeordnet der **SchoolModel.Student** Typ und die **PersonID** und **Registrierung** Spalten der **Person** Tabelle.

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

## <a name="deletefunction-element-msl"></a>DeleteFunction-Element (MSL)

Die **DeleteFunction** -Element der mapping-Spezifikationssprache (MSL) ordnet die Löschfunktion eines Entitätstyps oder einer Zuordnung im konzeptionellen Modell einer gespeicherten Prozedur in der zugrunde liegenden Datenbank. Gespeicherte Prozeduren, denen Änderungsfunktionen zugeordnet werden, müssen im Speichermodell deklariert werden. Weitere Informationen finden Sie in der Function-Element (SSDL).

> [!NOTE]
> Wenn Sie nicht zuordnen alle drei Einfüge-, update oder delete-Vorgänge eines Entitätstyps zu gespeicherten Prozeduren, wird die nicht zugeordneten Vorgänge fehl, wenn zur Laufzeit ausgeführt und ein "UpdateException" ausgelöst.

### <a name="deletefunction-applied-to-entitytypemapping"></a>DeleteFunction angewendet auf EntityTypeMapping

Bei Anwendung auf die EntityTypeMapping-Element, das **DeleteFunction** Element wird die Löschfunktion eines Entitätstyps im konzeptionellen Modell einer gespeicherten Prozedur zugeordnet.

Die **DeleteFunction** Element haben die folgenden untergeordneten Elemente bei Anwendung auf eine **EntityTypeMapping** Element:

-   AssociationEnd (null oder mehr)
-   ComplexProperty (null oder mehr)
-   ScalarProperty (null oder mehr)

#### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **DeleteFunction** -Element, wenn es gilt eine **EntityTypeMapping** Element.

| Attributname            | Ist erforderlich | Wert                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Funktionsname**          | Ja         | Der mit einem Namespace qualifizierte Name der gespeicherten Prozedur, der die Löschfunktion zugeordnet wird. Die gespeicherte Prozedur muss im Speichermodell deklariert werden. |
| **RowsAffectedParameter** | Nein          | Der Name des Ausgabeparameters, der die Anzahl der betroffenen Zeilen zurückgibt.                                                                               |

#### <a name="example"></a>Beispiel

Das folgende Beispiel basiert auf dem Modell "School" und zeigt die **DeleteFunction** -Element die Löschfunktion Zuordnen der **Person** Entitätstyp, der die **DeletePerson** gespeicherte Prozedur. Die **DeletePerson** wird die gespeicherte Prozedur im Speichermodell deklariert.

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

### <a name="deletefunction-applied-to-associationsetmapping"></a>DeleteFunction angewendet auf AssociationSetMapping

Bei Anwendung auf das AssociationSetMapping-Element, das **DeleteFunction** Element wird die Löschfunktion einer Zuordnung im konzeptionellen Modell einer gespeicherten Prozedur zugeordnet.

Die **DeleteFunction** Element haben die folgenden untergeordneten Elemente bei Anwendung auf die **AssociationSetMapping** Element:

-   EndProperty

#### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **DeleteFunction** -Element, wenn es gilt der **AssociationSetMapping** Element.

| Attributname            | Ist erforderlich | Wert                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Funktionsname**          | Ja         | Der mit einem Namespace qualifizierte Name der gespeicherten Prozedur, der die Löschfunktion zugeordnet wird. Die gespeicherte Prozedur muss im Speichermodell deklariert werden. |
| **RowsAffectedParameter** | Nein          | Der Name des Ausgabeparameters, der die Anzahl der betroffenen Zeilen zurückgibt.                                                                               |

#### <a name="example"></a>Beispiel

Das folgende Beispiel basiert auf dem Modell "School" und zeigt die **DeleteFunction** Element verwendet, um die Löschfunktion Zuordnen der **CourseInstructor** Zuordnung, die die  **DeleteCourseInstructor** gespeicherte Prozedur. Die **DeleteCourseInstructor** wird die gespeicherte Prozedur im Speichermodell deklariert.

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

## <a name="endproperty-element-msl"></a>EndProperty-Element (MSL)

Die **EndProperty** -Element der mapping-Spezifikationssprache (MSL) definiert die Zuordnung eines Endes oder einer Änderungsfunktion einer konzeptionellen Modellzuordnung zu der zugrunde liegenden Datenbank. Die Eigenschaftenspalte Zuordnung wird in einem untergeordneten ScalarProperty-Element angegeben.

Wenn ein **EndProperty** Element wird verwendet, um die Zuordnung für das Ende einer konzeptionellen Modellzuordnung zu definieren, es ist ein untergeordnetes Element eines AssociationSetMapping-Elements. Wenn die **EndProperty** Element wird verwendet, um die Zuordnung für eine Änderungsfunktion einer konzeptionellen Modellzuordnung zu definieren, es ist ein untergeordnetes Element eines InsertFunction-Element oder DeleteFunction-Element.

Die **EndProperty** -Element kann die folgenden untergeordneten Elemente aufweisen:

-   ScalarProperty (null oder mehr)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute für die **EndProperty** Element:

| Attributname | Ist erforderlich | Wert                                                 |
|:---------------|:------------|:------------------------------------------------------|
| name           | Ja         | Der Name des Zuordnungsendes, das zugeordnet wird. |

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **AssociationSetMapping** Element, in dem die **FK\_Kurs\_Abteilung** der ZuordnungimkonzeptionellenModellzugeordnetist**Kurs** Tabelle in der Datenbank. Zuordnungen zwischen zuordnungstypeigenschaften zu Tabellenspalten werden in untergeordneten angegeben **EndProperty** Elemente.

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

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt die **EndProperty** Element mapping einer Zuordnung der INSERT- und Delete-Funktionen (**CourseInstructor**) gespeicherten Prozeduren in der zugrunde liegenden Datenbank. Die Funktionen, denen sie zugeordnet werden, sind im Speichermodell deklariert.

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

## <a name="entitycontainermapping-element-msl"></a>EntityContainerMapping-Element (MSL)

Die **EntityContainerMapping** -Element der mapping-Spezifikationssprache (MSL) ordnet den Entitätencontainer im konzeptionellen Modell der Entitätscontainer im Speichermodell. Die **EntityContainerMapping** -Element der Mapping-Element untergeordnet ist.

Die **EntityContainerMapping** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   EntitySetMapping (null oder mehr)
-   AssociationSetMapping (null oder mehr)
-   FunctionImportMapping (null oder mehr)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **EntityContainerMapping** Element.

| Attributname            | Ist erforderlich | Wert                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StorageModelContainer** | Ja         | Der Name des Entitätscontainers im Speichermodell, der zugeordnet wird.                                                                                                                                                                                     |
| **CdmEntityContainer**    | Ja         | Der Name des Entitätscontainers im konzeptionellen Modell, der zugeordnet wird.                                                                                                                                                                                  |
| **GenerateUpdateViews**   | Nein          | **"True"** oder **"false"**. Wenn **"false"**, es werden keine Updateansichten generiert. Dieses Attribut sollte festgelegt werden, um **"false"** Wenn Sie eine schreibgeschützte Zuordnung, die ungültig wäre, da Daten möglicherweise erfolgreich Fällen kein Roundtrip ausgeführt haben. <br/> Der Standardwert ist **"true"**. |

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **EntityContainerMapping** Element, zugeordnet ist, die **SchoolModelEntities** Container (der Entitätscontainer des konzeptionellen Modells), die  **SchoolModelStoreContainer** Container (das Speichermodell-Entitätscontainer):

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

## <a name="entitysetmapping-element-msl"></a>EntitySetMapping-Element (MSL)

Die **EntitySetMapping** -Element in mapping Specification Language (MSL) ordnet alle Typen in einem konzeptionellen modellentität auf der Entität im Speichermodell legt. Eine Entitätenmenge im konzeptionellen Modell ist ein logischer Container für Instanzen der Entitäten des gleichen Typs (und abgeleitete Typen). Eine Entitätenmenge im Speichermodell stellt eine Tabelle oder Sicht in der zugrunde liegenden Datenbank dar. Entitätenmenge im konzeptionellen Modell wird angegeben, indem der Wert des der **Namen** Attribut der **EntitySetMapping** Element. Die zugeordnete Tabelle oder Sicht angegeben ist, indem die **StoreEntitySet** Attributs in jedem untergeordneten MappingFragment-Element oder in der **EntitySetMapping** Element selbst.

Die **EntitySetMapping** -Element kann die folgenden untergeordneten Elemente aufweisen:

-   EntityTypeMapping (null oder mehr)
-   QueryView (0 (null) oder 1)
-   MappingFragment (null oder mehr)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **EntitySetMapping** Element.

| Attributname           | Ist erforderlich | Wert                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                 | Ja         | Der Name der Entitätenmenge im konzeptionellen Modell, die zugeordnet wird.                                                                                                                                                             |
| **TypeName** **1**       | Nein          | Der Name des Entitätstyp im konzeptionellen Modell, der zugeordnet wird.                                                                                                                                                            |
| **StoreEntitySet** **1** | Nein          | Der Name der Entitätenmenge im Speichermodell, die zugeordnet wird.                                                                                                                                                             |
| **Das MakeColumnsDistinct**  | Nein          | **"True"** oder **"false"** abhängig davon, ob nur unterschiedliche Zeilen zurückgegeben werden. <br/> Wenn dieses Attribut, um festgelegt wird **"true"**, **GenerateUpdateViews** Attribut des EntityContainerMapping-Elements muss festgelegt werden, um **"false"**. |

 

**1** der **TypeName** und **StoreEntitySet** Attribute können anstelle der EntityTypeMapping- und MappingFragment untergeordnete Elemente verwendet werden, um eine einzelne Tabelle einen einzelnen Entitätstyp zuzuordnen.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **EntitySetMapping** -Element, das in drei Typen (Basistyp und zwei abgeleitete Typen) zugeordnet ist die **Kurse** Entitätenmenge des konzeptionellen Modells drei verschiedenen Tabellen in der zugrunde liegende Datenbank. Die Tabellen werden angegeben, indem die **StoreEntitySet** -Attribut in jedem **MappingFragment** Element.

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

## <a name="entitytypemapping-element-msl"></a>EntityTypeMapping-Element (MSL)

Die **EntityTypeMapping** -Element der mapping-Spezifikationssprache (MSL) definiert die Zuordnung zwischen einen Entitätstyp im konzeptionellen Modell und Tabellen oder Sichten in der zugrunde liegenden Datenbank. Informationen zu den Entitätstypen des konzeptionellen Modells und zugrunde liegenden Datenbanktabellen oder-Ansichten finden Sie unter EntityType-Element (CSDL) und der EntitySet-Element (SSDL). Wird der Typ des konzeptionellen Modells Entität, die zugeordnet wird angegeben, indem die **TypeName** Attribut der **EntityTypeMapping** Element. Der Tabelle oder Sicht, die zugeordnet wird, wird angegeben durch die **StoreEntitySet** Attribut des untergeordneten MappingFragment-Elements.

Das untergeordnete Element kann verwendet werden, zum Zuordnen von Einfüge-, ModificationFunctionMapping aktualisieren oder Löschfunktionen von Entitätstypen zu gespeicherten Prozeduren in der Datenbank.

Die **EntityTypeMapping** -Element kann die folgenden untergeordneten Elemente aufweisen:

-   MappingFragment (null oder mehr)
-   ModificationFunctionMapping (0 (null) oder 1)
-   ScalarProperty
-   Bedingung

> [!NOTE]
> **MappingFragment** und **ModificationFunctionMapping** Elementen handelt es sich nicht um untergeordnete Elemente der **EntityTypeMapping** Element zur gleichen Zeit.


> [!NOTE]
> Die **ScalarProperty** und **Bedingung** Elemente können nur untergeordnete Elemente werden der **EntityTypeMapping** -Element, wenn er in ein FunctionImportMapping-Element verwendet wird.

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **EntityTypeMapping** Element.

| Attributname | Ist erforderlich | Wert                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Typname**   | Ja         | Der mit einem Namespace qualifizierte Name des Entitätstyps des konzeptionellen Modells, der zugeordnet wird. <br/> Wenn der Typ abstrakt oder ein abgeleiteter Typ ist, muss der Wert `IsOfType(Namespace-qualified_type_name)` lauten. |

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt ein EntitySetMapping-Element mit zwei untergeordneten **EntityTypeMapping** Elemente. In der ersten **EntityTypeMapping** -Element, das **SchoolModel.Person** Entitätstyp zugeordnet ist die **Person** Tabelle. In der zweiten **EntityTypeMapping** -Element, das die Aktualisierungsfunktion der **SchoolModel.Person** Typ einer gespeicherten Prozedur zugeordnet ist **UpdatePerson**, in der Datenbank .

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

### <a name="example"></a>Beispiel

Im nächsten Beispiel wird die Zuordnung einer Typhierarchie, in der der Stammtyp abstrakt ist, veranschaulicht. Beachten Sie die Verwendung der `IsOfType` Syntax für die **TypeName** Attribute.

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

## <a name="functionimportmapping-element-msl"></a>FunctionImportMapping-Element (MSL)

Die **FunctionImportMapping** -Element der mapping-Spezifikationssprache (MSL) definiert die Zuordnung zwischen einem Funktionsimport im konzeptionellen Modell und einer gespeicherten Prozedur oder Funktion in der zugrunde liegenden Datenbank. Funktionsimporte müssen im konzeptionellen Modell und gespeicherte Prozeduren müssen im Speichermodell deklariert werden. Weitere Informationen finden Sie unter FunctionImport-Element (CSDL) und Function-Element (SSDL).

> [!NOTE]
> Wenn ein Funktionsimport einen Entitätstyp des konzeptionellen Modells oder einen komplexen Typ zurückgibt, dann müssen standardmäßig die Namen der Spalten, die von der zugrunde liegenden gespeicherten Prozedur zurückgegeben werden, exakt den Namen der Eigenschaften des konzeptionellen Modelltyps entsprechen. Wenn die Spaltennamen nicht genau mit die Eigenschaftennamen übereinstimmen, muss die Zuordnung in einem ResultMapping-Element definiert werden.

Die **FunctionImportMapping** -Element kann die folgenden untergeordneten Elemente aufweisen:

-   ResultMapping (null oder mehr)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute für die **FunctionImportMapping** Element:

| Attributname         | Ist erforderlich | Wert                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| **FunctionImportName** | Ja         | Der Name des Funktionsimports im konzeptionellen Modell, der zugeordnet wird.           |
| **Funktionsname**       | Ja         | Der mit einem Namespace qualifizierte Name der Funktion im Speichermodell, die zugeordnet wird. |

### <a name="example"></a>Beispiel

Das folgende Beispiel basiert auf dem Modell "School". Betrachten Sie die folgende Funktion im Speichermodell:

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

Beachten Sie auch diesen Funktionsimport im konzeptionellen Modell:

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

Das folgende Beispiel zeigt eine **FunctionImportMapping** , das zum Zuordnen der Funktion und der Funktionsimport oben miteinander verwendet:

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a>InsertFunction-Element (MSL)

Die **InsertFunction** -Element der mapping-Spezifikationssprache (MSL) ordnet die Einfügefunktion eines Entitätstyps oder einer Zuordnung im konzeptionellen Modell einer gespeicherten Prozedur in der zugrunde liegenden Datenbank. Gespeicherte Prozeduren, denen Änderungsfunktionen zugeordnet werden, müssen im Speichermodell deklariert werden. Weitere Informationen finden Sie in der Function-Element (SSDL).

> [!NOTE]
> Wenn Sie nicht zuordnen alle drei Einfüge-, update oder delete-Vorgänge eines Entitätstyps zu gespeicherten Prozeduren, wird die nicht zugeordneten Vorgänge fehl, wenn zur Laufzeit ausgeführt und ein "UpdateException" ausgelöst.

Die **InsertFunction** Element kann ein untergeordnetes Element des ModificationFunctionMapping-Element und angewendet auf EntityTypeMapping-Element oder die AssociationSetMapping-Element.

### <a name="insertfunction-applied-to-entitytypemapping"></a>InsertFunction angewendet auf EntityTypeMapping

Bei Anwendung auf die EntityTypeMapping-Element, das **InsertFunction** -Element ordnet die Einfügefunktion eines Entitätstyps im konzeptionellen Modell einer gespeicherten Prozedur.

Die **InsertFunction** Element haben die folgenden untergeordneten Elemente bei Anwendung auf eine **EntityTypeMapping** Element:

-   AssociationEnd (null oder mehr)
-   ComplexProperty (null oder mehr)
-   ResultBinding (0 (null) oder 1)
-   ScalarProperty (null oder mehr)

#### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **InsertFunction** Element bei Anwendung auf eine **EntityTypeMapping** Element.

| Attributname            | Ist erforderlich | Wert                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Funktionsname**          | Ja         | Der mit einem Namespace qualifizierte Name der gespeicherten Prozedur, der die Einfügefunktion zugeordnet wird. Die gespeicherte Prozedur muss im Speichermodell deklariert werden. |
| **RowsAffectedParameter** | Nein          | Der Name des Ausgabeparameters, der die Anzahl der betroffenen Zeilen zurückgibt.                                                                               |

#### <a name="example"></a>Beispiel

Das folgende Beispiel basiert auf dem Modell "School" und zeigt die **InsertFunction** Element verwendet, um die Einfügefunktion des Entitätstyps Person zum Zuordnen der **InsertPerson** gespeicherte Prozedur. Die **InsertPerson** wird die gespeicherte Prozedur im Speichermodell deklariert.

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
### <a name="insertfunction-applied-to-associationsetmapping"></a>InsertFunction angewendet auf AssociationSetMapping

Bei Anwendung auf das AssociationSetMapping-Element, das **InsertFunction** -Element ordnet die Einfügefunktion einer Zuordnung im konzeptionellen Modell einer gespeicherten Prozedur.

Die **InsertFunction** Element haben die folgenden untergeordneten Elemente bei Anwendung auf die **AssociationSetMapping** Element:

-   EndProperty

#### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **InsertFunction** -Element, wenn es gilt der **AssociationSetMapping** Element.

| Attributname            | Ist erforderlich | Wert                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Funktionsname**          | Ja         | Der mit einem Namespace qualifizierte Name der gespeicherten Prozedur, der die Einfügefunktion zugeordnet wird. Die gespeicherte Prozedur muss im Speichermodell deklariert werden. |
| **RowsAffectedParameter** | Nein          | Der Name des Ausgabeparameters, der die Anzahl der betroffenen Zeilen zurückgibt.                                                                               |

#### <a name="example"></a>Beispiel

Das folgende Beispiel basiert auf dem Modell "School" und zeigt die **InsertFunction** Element verwendet, um die Einfügefunktion Zuordnen der **CourseInstructor** Zuordnung, die die  **InsertCourseInstructor** gespeicherte Prozedur. Die **InsertCourseInstructor** wird die gespeicherte Prozedur im Speichermodell deklariert.

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

## <a name="mapping-element-msl"></a>Mapping-Element (MSL)

Die **Zuordnung** -Element der mapping-Spezifikationssprache (MSL) enthält Informationen zum Zuordnen von Objekten, die in einem konzeptionellen Modell mit einer Datenbank definiert sind (wie in einem Speichermodell beschrieben). Weitere Informationen finden Sie in der CSDL-Spezifikation und SSDL-Spezifikation.

Die **Zuordnung** Element ist das Stammelement für die Mapping-Spezifikation. Der XML-Namespace für mapping-Spezifikationen http://schemas.microsoft.com/ado/2009/11/mapping/cs.

Das Mapping-Element kann die folgenden untergeordneten Elemente aufweisen (der vorliegenden Reihenfolge entsprechend):

-   Alias (null oder mehr)
-   EntityContainerMapping (genau ein Element)

Die Namen aller Typen des konzeptionellen Modells und Typen des Speichermodells, auf die in MSL verwiesen wird, müssen mit dem jeweiligen Namespacenamen qualifiziert werden. Informationen zu den Namespacenamen des konzeptionellen Modells finden Sie in der Schema-Element (CSDL). Informationen zu den Namespacenamen des Storage-Modell finden Sie in der Schema-Element (SSDL). Aliasnamen für Namespaces, die in MSL verwendet werden, können mit dem Alias-Element definiert werden.

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Zuordnung** Element.

| Attributname | Ist erforderlich | Wert                                                 |
|:---------------|:------------|:------------------------------------------------------|
| **LEERTASTE**      | Ja         | **C-S-**. Dies ist ein fester Wert, der nicht geändert werden kann. |

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **Zuordnung** -Element, das Teil des Modells "School" basiert. Weitere Informationen über das Modell "School" finden Sie in Schnellstart (Entity Framework):

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

## <a name="mappingfragment-element-msl"></a>MappingFragment-Element (MSL)

Die **MappingFragment** -Element der mapping-Spezifikationssprache (MSL) definiert die Zuordnung zwischen den Eigenschaften einen Entitätstyp des konzeptionellen Modells und einer Tabelle oder Sicht in der Datenbank. Informationen zu den Entitätstypen des konzeptionellen Modells und zugrunde liegenden Datenbanktabellen oder-Ansichten finden Sie unter EntityType-Element (CSDL) und der EntitySet-Element (SSDL). Die **MappingFragment** kann ein untergeordnetes Element des EntityTypeMapping-Element oder das EntitySetMapping-Element sein.

Die **MappingFragment** -Element kann die folgenden untergeordneten Elemente aufweisen:

-   ComplexType (null oder mehr)
-   ScalarProperty (null oder mehr)
-   Bedingung (null oder mehr)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **MappingFragment** Element.

| Attributname          | Ist erforderlich | Wert                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StoreEntitySet**      | Ja         | Der Name der Tabelle oder Ansicht, die zugeordnet wird.                                                                                                                                                                           |
| **Das MakeColumnsDistinct** | Nein          | **"True"** oder **"false"** abhängig davon, ob nur unterschiedliche Zeilen zurückgegeben werden. <br/> Wenn dieses Attribut, um festgelegt wird **"true"**, **GenerateUpdateViews** Attribut des EntityContainerMapping-Elements muss festgelegt werden, um **"false"**. |

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **MappingFragment** Element als untergeordnetes Element eine **EntityTypeMapping** Element. In diesem Beispiel ist die Eigenschaften der **Kurs** Spalten des Typs im konzeptionellen Modell zugeordnet sind die **Kurs** Tabelle in der Datenbank.

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

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **MappingFragment** Element als untergeordnetes Element eine **EntitySetMapping** Element. Wie im Beispiel oben "," Eigenschaften der **Kurs** Spalten des Typs im konzeptionellen Modell zugeordnet sind die **Kurs** Tabelle in der Datenbank.

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

## <a name="modificationfunctionmapping-element-msl"></a>ModificationFunctionMapping-Element (MSL)

Die **ModificationFunctionMapping** -Element der mapping-Spezifikationssprache (MSL) ordnet das Einfügen, aktualisieren und Löschfunktionen eines Entitätstyps zu gespeicherten Prozeduren in der zugrunde liegenden Datenbank konzeptionellen Modell. Die **ModificationFunctionMapping** Element auch ordnen Sie die Insert und delete-Funktionen für m: n Zuordnungen im konzeptionellen Modell gespeicherten Prozeduren in der zugrunde liegenden Datenbank. Gespeicherte Prozeduren, denen Änderungsfunktionen zugeordnet werden, müssen im Speichermodell deklariert werden. Weitere Informationen finden Sie in der Function-Element (SSDL).

> [!NOTE]
> Wenn Sie nicht zuordnen alle drei Einfüge-, update oder delete-Vorgänge eines Entitätstyps zu gespeicherten Prozeduren, wird die nicht zugeordneten Vorgänge fehl, wenn zur Laufzeit ausgeführt und ein "UpdateException" ausgelöst.


> [!NOTE]
> Wenn die Änderungsfunktionen für eine Entität in einer Vererbungshierarchie gespeicherten Prozeduren zugeordnet werden, dann müssen die Änderungsfunktionen für alle Typen in der Hierarchie gespeicherten Prozeduren zugeordnet werden.

Die **ModificationFunctionMapping** Element kann ein untergeordnetes Element des EntityTypeMapping-Element oder das AssociationSetMapping-Element sein.

Die **ModificationFunctionMapping** -Element kann die folgenden untergeordneten Elemente aufweisen:

-   DeleteFunction (0 (null) oder 1)
-   InsertFunction (0 (null) oder 1)
-   UpdateFunction (0 (null) oder 1)

Es sind keine Attribute für die **ModificationFunctionMapping** Element.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt die Entitätenmenge für die Zuordnung der **Personen** Entitätenmenge im Modell "School". Zusätzlich zur spaltenzuordnung für die **Person** Entitätstyp, der die Zuordnung der INSERT-, aktualisieren und löschen Sie die Funktionen des die **Person** angezeigt werden. Die Funktionen, denen sie zugeordnet werden, sind im Speichermodell deklariert.

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

### <a name="example"></a>Beispiel

Im folgende Beispiel wird gezeigt, wie für die Zuordnung der **CourseInstructor** Zuordnungssatz in das Modell "School". Zusätzlich zur spaltenzuordnung für die **CourseInstructor** -Zuordnung wird die Zuordnung der INSERT- und Delete-Funktionen des die **CourseInstructor** -Zuordnung gezeigt. Die Funktionen, denen sie zugeordnet werden, sind im Speichermodell deklariert.

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
 

 

## <a name="queryview-element-msl"></a>QueryView-Element (MSL)

Die **QueryView** -Element der mapping-Spezifikationssprache (MSL) definiert eine schreibgeschützte Zuordnung eines Entitätstyp oder einer Zuordnung im konzeptionellen Modell und eine Tabelle in der zugrunde liegenden Datenbank. Die Zuordnung wird definiert, mit einer Entity SQL-Abfrage, die für das Speichermodell ausgewertet wird, und das Resultset im Hinblick auf eine Entität oder eine Zuordnung im konzeptionellen Modell ausgedrückt. Da Abfragesichten schreibgeschützt sind, können die durch Abfragesichten definierten Typen nicht mit herkömmlichen Aktualisierungsbefehlen aktualisiert werden. Diese Typen können mithilfe von Änderungsfunktionen aktualisiert werden. Weitere Informationen finden Sie unter Vorgehensweise: Zuordnen von Änderungsfunktionen zu gespeicherten Prozeduren.

> [!NOTE]
> In der **QueryView** -Element, Entity SQL-Ausdrücke, die enthalten **GroupBy**, gruppenaggregate und Navigationseigenschaften werden nicht unterstützt.

 

Die **QueryView** Element kann ein untergeordnetes Element des EntitySetMapping-Element oder das AssociationSetMapping-Element sein. Im ersten Fall definiert die Abfrageansicht eine schreibgeschützte Zuordnung für eine Entität im konzeptionellen Modell. Im zweiten Fall definiert die Abfrageansicht eine schreibgeschützte Zuordnung für eine Zuordnung im konzeptionellen Modell.

> [!NOTE]
> Wenn die **AssociationSetMapping** Element ist für eine Zuordnung mit einer referenziellen Einschränkung, die **AssociationSetMapping** Element wird ignoriert. Weitere Informationen finden Sie unter ReferentialConstraint-Element (CSDL).

Die **QueryView** Element kann keine untergeordneten Elemente aufweisen.

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **QueryView** Element.

| Attributname | Ist erforderlich | Wert                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **Typname**   | Nein          | Der Name des konzeptionellen Modelltyps, der durch die Abfrageansicht zugeordnet wird. |

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt die **QueryView** -Element als untergeordnetes Element des der **EntitySetMapping** Element und definiert eine abfrageansichtszuordnung für den **Abteilung** Entitätstyp in der Modell "School".

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

Da die Abfrage nur eine Teilmenge der Elemente zurückgibt. die **Abteilung** Typ im Speichermodell, die **Abteilung** Typ im Modell "School" basierend auf dieser Zuordnung wie folgt geändert wurde:

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

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt die **QueryView** Element als untergeordnetes Element eine **AssociationSetMapping** Element und definiert eine schreibgeschützte Zuordnung für die `FK_Course_Department` -Zuordnung im Modell "School".

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
 
### <a name="comments"></a>Kommentare

Sie können Abfragesichten definieren, um die folgenden Szenarien zu ermöglichen:

-   Definieren einer Entität im konzeptionellen Modell, die nicht alle Eigenschaften der Entität im Speichermodell einschließt. Dazu gehören Eigenschaften, die keine Standardwerte und unterstützen keine **null** Werte.
-   Zuordnen von berechneten Spalten im Speichermodell zu Eigenschaften von Entitätstypen im konzeptionellen Modell.
-   Definieren eines Mappings, bei dem die Bedingungen, die für die Partitionierung von Entitäten im konzeptionellen Modell verwendet werden, nicht auf Gleichheit basieren. Wenn Sie angeben, ein bedingtes Mapping mithilfe der **Bedingung** Element die angegebene Bedingung muss gleich dem angegebenen Wert. Weitere Informationen finden Sie in der Condition-Element (MSL).
-   Zuordnen derselben Spalte im Speichermodell zu mehreren Typen im konzeptionellen Modell.
-   Zuordnen mehrerer Typen zu derselben Tabelle.
-   Definieren von Zuordnungen im konzeptionellen Modell, die nicht auf Fremdschlüsseln im relationalen Schema basieren.
-   Verwenden benutzerdefinierter Geschäftslogik, um die Werte von Eigenschaften im konzeptionellen Modell festzulegen. Beispielsweise könnten Sie den Zeichenfolgenwert "T" in der Datenquelle auf einen Wert des zuordnen **"true"**, ein boolescher Wert, die im konzeptionellen Modell.
-   Definieren bedingter Filter für Abfrageergebnisse.
-   Erzwingen von geringeren Dateneinschränkungen im konzeptionellen Modell als im Speichermodell. Angenommen, Sie konnten, eine Eigenschaft im konzeptionellen Modell NULL-Werte zulässt, selbst wenn die Spalte, der sie zugeordnet ist, nicht unterstützt **null**Werte.

Die folgenden Aspekte gelten, wenn Sie Abfragesichten für Entitäten definieren:

-   Abfragesichten sind schreibgeschützt. Sie können Entitäten nur mit Änderungsfunktionen aktualisieren.
-   Wenn Sie eine Entität durch eine Abfragesicht definieren, müssen Sie auch alle verknüpften Entitäten durch Abfragesichten definieren.
-   Wenn Sie eine m: n Zuordnung zu einer Entität im Speichermodell, die eine Linktabelle im relationalen Schema darstellt zuordnen, müssen Sie definieren eine **QueryView** Element in der **AssociationSetMapping** -Element für diese Linktabelle.
-   Abfragesichten müssen für alle Typen in einer Typhierarchie definiert werden. Dazu stehen Ihnen folgende Möglichkeiten zur Verfügung:
-   -   Mit einem einzelnen **QueryView** -Element, das eine Entity SQL-Abfrage gibt an, die eine Union aller Entitätstypen in der Hierarchie zurückgibt.
    -   Mit einem einzelnen **QueryView** -Element, das eine Entity SQL-Abfrage gibt an, die die Groß-/KLEINSCHREIBUNG-Operator verwendet, um einen bestimmten Typ in der Hierarchie zurückzugeben auf Grundlage einer bestimmten Bedingung.
    -   Mit einem zusätzlichen **QueryView** -Element für einen bestimmten Typ in der Hierarchie. In diesem Fall verwenden Sie die **TypeName** Attribut der **QueryView** Element, um den Entitätstyp für jede Sicht anzugeben.
-   Wenn eine Abfragesicht definiert ist, können Sie nicht angeben der **StorageSetName** -Attribut für die **EntitySetMapping** Element.
-   Wenn eine Abfragesicht definiert ist, die **EntitySetMapping**Element kann nicht gleichzeitig enthalten **Eigenschaft** Zuordnungen.

## <a name="resultbinding-element-msl"></a>ResultBinding-Element (MSL)

Die **ResultBinding** -Element der mapping-Spezifikationssprache (MSL) ordnet SpalteWerte, die zurückgegeben werden von gespeicherten Prozeduren, Entitätseigenschaften im konzeptionellen Modell beim Entitätstyp ändernden Funktionen zugeordnet werden an die gespeicherte die Prozeduren in der zugrunde liegenden Datenbank. Gespeicherte Prozedur z. B. wenn der Wert der Identitätsspalte zurückgegeben wird, von einer Insert die **ResultBinding** Element wird den zurückgegebenen Wert einer Entitätstypeigenschaft im konzeptionellen Modell zugeordnet.

Die **ResultBinding** Element kann ein untergeordnetes Element der InsertFunction-Element oder die UpdateFunction-Element sein.

Die **ResultBinding** Element kann keine untergeordneten Elemente aufweisen.

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute für die **ResultBinding** Element:

| Attributname | Ist erforderlich | Wert                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **Name**       | Ja         | Der Name der Entitätseigenschaft im konzeptionellen Modell, die zugeordnet wird. |
| **Spaltenname** | Ja         | Der Name der Spalte, die zugeordnet wird.                                          |

### <a name="example"></a>Beispiel

Das folgende Beispiel basiert auf dem Modell "School" und zeigt eine **InsertFunction** Element verwendet, um die Einfügefunktion zuzuordnen der **Person** Entitätstyp, der die **InsertPerson** gespeicherte Prozedur. (Die **InsertPerson** gespeicherte Prozedur wird im folgenden dargestellt und wird im Speichermodell deklariert.) Ein **ResultBinding** -Element wird verwendet, um einen Spaltenwert zuzuordnen, die von der gespeicherten Prozedur zurückgegeben wird (**NewPersonID**), einer Entitätstypeigenschaft (**PersonID**).

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

Die folgende Transact-SQL wird beschrieben, die **InsertPerson** gespeicherte Prozedur:

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

## <a name="resultmapping-element-msl"></a>ResultMapping-Element (MSL)

Die **ResultMapping** -Element der mapping-Spezifikationssprache (MSL) definiert die Zuordnung zwischen einem Funktionsimport im konzeptionellen Modell und eine gespeicherte Prozedur in der zugrunde liegenden Datenbank, wenn Folgendes zutrifft:

-   Der Funktionsimport gibt einen Entitätstyp des konzeptionellen Modells oder einen komplexen Typ zurück.
-   Die Namen der Spalten, die von der gespeicherten Prozedur zurückgegeben werden, entsprechen nicht exakt den Namen der Eigenschaften für den Entitätstyp oder den komplexem Typ.

Standardmäßig basiert die Zuordnung der von einer gespeicherten Prozedur zurückgegebenen Spalten zu einem Entitätstyp oder einem komplexen Typ auf den Spalten- und Eigenschaftennamen. Wenn Spaltennamen nicht exakt Eigenschaftennamen übereinstimmen, müssen Sie verwenden die **ResultMapping** Element, um die Zuordnung zu definieren. Ein Beispiel für die standardzuordnung finden Sie unter FunctionImportMapping-Element (MSL).

Die **ResultMapping** Element ist ein untergeordnetes Element des FunctionImportMapping-Elements.

Die **ResultMapping** -Element kann die folgenden untergeordneten Elemente aufweisen:

-   EntityTypeMapping (null oder mehr)
-   ComplexTypeMapping

Es sind keine Attribute für die **ResultMapping** Element.

### <a name="example"></a>Beispiel

Betrachten Sie die folgende gespeicherte Prozedur:

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

Betrachten Sie auch den folgenden Entitätstyp des konzeptionellen Modells:

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

Um einen Funktionsimport zu erstellen, die Instanzen des vorherigen Entitätstyps zurückgibt, die Zuordnung zwischen den Spalten, die von der gespeicherten Prozedur zurückgegeben, und der Entitätstyp muss definiert werden, einem **ResultMapping** Element:

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

## <a name="scalarproperty-element-msl"></a>ScalarProperty-Element (MSL)

Die **ScalarProperty** -Element der mapping-Spezifikationssprache (MSL) ordnet eine Eigenschaft auf einen Entitätstyp des konzeptionellen Modells, den komplexen Typ oder die Zuordnung einer Tabellenspalte oder der Parameter der gespeicherten Prozedur in der zugrunde liegenden Datenbank.

> [!NOTE]
> Gespeicherte Prozeduren, denen Änderungsfunktionen zugeordnet werden, müssen im Speichermodell deklariert werden. Weitere Informationen finden Sie in der Function-Element (SSDL).

Die **ScalarProperty** Element kann ein untergeordnetes Element der folgenden Elemente sein:

-   MappingFragment
-   InsertFunction
-   UpdateFunction
-   DeleteFunction
-   EndProperty
-   ComplexProperty
-   ResultMapping

Als untergeordnetes Element der **MappingFragment**, **ComplexProperty**, oder **EndProperty** -Element, das **ScalarProperty** -Element ordnet eine Eigenschaft im konzeptionellen Modell an eine Spalte in der Datenbank. Als untergeordnetes Element der **InsertFunction**, **UpdateFunction**, oder **DeleteFunction** -Element, das **ScalarProperty** -Element ordnet eine Eigenschaft im konzeptionellen Modell Parameter einer gespeicherten Prozedur.

Die **ScalarProperty** Element kann keine untergeordneten Elemente aufweisen.

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die Attribute, die zum Anwenden der **ScalarProperty** Element unterscheiden sich abhängig von der Rolle des Elements.

Die folgende Tabelle beschreibt die Attribute, die gelten, dass bei der **ScalarProperty** Element wird verwendet, um eine Eigenschaft des konzeptionellen Modells einer Spalte in der Datenbank zuzuordnen:

| Attributname | Ist erforderlich | Wert                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Name**       | Ja         | Der Name der Eigenschaft im konzeptionellen Modell, die zugeordnet wird. |
| **Spaltenname** | Ja         | Der Name der Tabellenspalte, die zugeordnet wird.              |

Die folgende Tabelle beschreibt die Attribute für die **ScalarProperty** -Element, wenn es verwendet wird, um Parameter einer gespeicherten Prozedur eine Eigenschaft im konzeptionellen Modell zuzuordnen:

| Attributname    | Ist erforderlich | Wert                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**          | Ja         | Der Name der Eigenschaft im konzeptionellen Modell, die zugeordnet wird.                                                                                 |
| **ParameterName** | Ja         | Der Name des Parameters, der zugeordnet wird.                                                                                                 |
| **Version**       | Nein          | **Aktuelle** oder **ursprünglichen** abhängig davon, ob der aktuelle Wert oder den ursprünglichen Wert der Eigenschaft für parallelitätsüberprüfungen verwendet werden soll. |

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt die **ScalarProperty** Element, das verwendet wird, gibt es zwei Möglichkeiten:

-   Zum Zuordnen von den Eigenschaften von den **Person** Entitätstyp, der die Spalten der **Person**Tabelle.
-   Zum Zuordnen von den Eigenschaften von der **Person** Entitätstyp, der die Parameter der **UpdatePerson** gespeicherte Prozedur. Die gespeicherten Prozeduren werden im Speichermodell deklariert.

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

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt die **ScalarProperty** , das zum Zuordnen von Insert und delete-Funktionen, die von einer konzeptionellen Modellzuordnung zu gespeicherten Prozeduren in der Datenbank verwendet. Die gespeicherten Prozeduren werden im Speichermodell deklariert.

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

## <a name="updatefunction-element-msl"></a>UpdateFunction-Element (MSL)

Die **UpdateFunction** -Element der mapping-Spezifikationssprache (MSL) ordnet Aktualisierungs-und Löschfunktionen eines Entitätstyps im konzeptionellen Modell einer gespeicherten Prozedur in der zugrunde liegenden Datenbank. Gespeicherte Prozeduren, denen Änderungsfunktionen zugeordnet werden, müssen im Speichermodell deklariert werden. Weitere Informationen finden Sie in der Function-Element (SSDL).

> [!NOTE]
>  Wenn Sie nicht zuordnen alle drei Einfüge-, update oder delete-Vorgänge eines Entitätstyps zu gespeicherten Prozeduren, wird die nicht zugeordneten Vorgänge fehl, wenn zur Laufzeit ausgeführt und ein "UpdateException" ausgelöst.

Die **UpdateFunction** Element kann ein untergeordnetes Element des ModificationFunctionMapping-Element und angewendet auf EntityTypeMapping-Element.

Die **UpdateFunction** -Element kann die folgenden untergeordneten Elemente aufweisen:

-   AssociationEnd (null oder mehr)
-   ComplexProperty (null oder mehr)
-   ResultBinding (0 (null) oder 1)
-   ScalarProperty (null oder mehr)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **UpdateFunction** Element.

| Attributname            | Ist erforderlich | Wert                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Funktionsname**          | Ja         | Der mit einem Namespace qualifizierte Name der gespeicherten Prozedur, der die Aktualisierungsfunktion zugeordnet wird. Die gespeicherte Prozedur muss im Speichermodell deklariert werden. |
| **RowsAffectedParameter** | Nein          | Der Name des Ausgabeparameters, der die Anzahl der betroffenen Zeilen zurückgibt.                                                                               |

### <a name="example"></a>Beispiel

Das folgende Beispiel basiert auf dem Modell "School" und zeigt die **UpdateFunction** Element verwendet, um die Aktualisierungsfunktion der **Person** Entitätstyp, der die **UpdatePerson** gespeicherte Prozedur. Die **UpdatePerson** wird die gespeicherte Prozedur im Speichermodell deklariert.

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
