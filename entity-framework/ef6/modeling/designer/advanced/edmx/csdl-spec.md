---
title: CSDL-Spezifikation - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c54255f4-253f-49eb-bec8-ad7927ac2fa3
ms.openlocfilehash: 438af83b8a1ad51ee8414341181412e950d0e117
ms.sourcegitcommit: 29f928a6116771fe78f306846e6f2d45cbe8d1f4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460149"
---
# <a name="csdl-specification"></a>CSDL-Spezifikation
Die konzeptionelle Schemadefinitionssprache (CSDL) ist eine XML-basierte Sprache, die die Entitäten, Beziehungen und Funktionen beschreibt, die ein konzeptionelles Modell einer datengesteuerten Anwendung bilden. Dieses konzeptionelle Modell kann vom Entity Framework oder WCF Data Services verwendet werden. Die Metadaten, die in CSDL beschrieben werden wird von Entity Framework zum Zuordnen von Entitäten und Beziehungen, die in einem konzeptionellen Modell an eine Datenquelle definiert sind. Weitere Informationen finden Sie unter [SSDL-Spezifikation](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md) und [MSL-Spezifikation](~/ef6/modeling/designer/advanced/edmx/msl-spec.md).

CSDL ist Entity Framework Implementierung des Entity Data Model.

In einer Entity Framework-Anwendung Metadaten des konzeptionellen Modells aus einer CSDL-Datei (geschrieben in CSDL) in eine Instanz von der System.Data.Metadata.Edm.EdmItemCollection geladen wird und kann mit Verwendung von Methoden in der System.Data.Metadata.Edm.MetadataWorkspace-Klasse. Entitätsframework verwendet Metadaten des konzeptionellen Modells, um Abfragen für das konzeptionelle Modell in datenquellenspezifische Befehle zu übersetzen.

Dem EF Designer speichert Informationen zum konzeptionellen Modell in einer EDMX-Datei zur Entwurfszeit. Zur Buildzeit verwendet dem EF Designer Informationen in einer EDMX-Datei aus, um die CSDL-Datei zu erstellen, die zur Laufzeit vom Entity Framework benötigt wird.

Die verschiedenen Versionen von CSDL werden von XML-Namespaces unterschieden.

| CSDL-Version | XML-Namespace                                |
|:-------------|:---------------------------------------------|
| CSDL v1      | http://schemas.microsoft.com/ado/2006/04/edm |
| CSDL-v2      | http://schemas.microsoft.com/ado/2008/09/edm |
| CSDL v3      | http://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a>Zuordnungselement (CSDL)

Ein **Zuordnung** -Element definiert eine Beziehung zwischen zwei Entitätstypen. Eine Zuordnung muss die Entitätstypen, die in der Beziehung enthalten sind, und die mögliche Anzahl von Entitätstypen an den Enden der Beziehung angeben, die auch als Multiplizität bezeichnet wird. Die Multiplizität eines zuordnungsendes kann einen Wert von eins (1), NULL oder eins (0.. 1) oder viele haben (\*). Diese Informationen sind in zwei untergeordnete Ende Elemente angegeben.

Auf Entitätstypinstanzen an einem Ende einer Zuordnung kann über Navigationseigenschaften oder Fremdschlüssel zugegriffen werden, sofern sie für einen Entitätstyp verfügbar gemacht werden.

In einer Anwendung stellt eine Instanz einer Zuordnung eine bestimmte Zuordnung zwischen Instanzen von Entitätstypen dar. Zuordnungsinstanzen werden logisch in einem Zuordnungssatz gruppiert.

Ein **Zuordnung** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element)
-   Ende (genau 2 Elemente)
-   ReferentialConstraint (0 (null) oder ein Element)
-   Anmerkungselemente (null oder mehr Elemente)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Zuordnung** Element.

| Attributname | Ist erforderlich | Wert                        |
|:---------------|:------------|:-----------------------------|
| **Name**       | Ja         | Der Name der Zuordnung. |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Zuordnung** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **Zuordnung** -Element, definiert der **CustomerOrders** Zuordnung, wenn Fremdschlüssel nicht auf verfügbar gemacht wurden die **Kunden** und  **Reihenfolge** Entitätstypen. Die **Multiplizität** Werte für die einzelnen **End** der Zuordnung angegeben, dass viele **Bestellungen** zugeordnet sein kann eine **Kunden**, aber nur ein **Kunden** zugeordnet sein kann ein **Reihenfolge**. Darüber hinaus die **OnDelete** Element gibt an, dass alle **Bestellungen** , beziehen sich auf eine bestimmte **Kunden** und in den geladen wurden der ObjectContext gelöscht werden Wenn die **Kunden** wird gelöscht.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

Das folgende Beispiel zeigt eine **Zuordnung** -Element, definiert der **CustomerOrders** Zuordnung, wenn der Fremdschlüssel auf verfügbar gemacht wurden die **Kunden** und  **Reihenfolge** Entitätstypen. Wurden Fremdschlüssel verfügbar gemacht, die die Beziehung zwischen den Entitäten Verwaltung erfolgt über eine **ReferentialConstraint** Element. Ein entsprechendes AssociationSetMapping-Element ist nicht erforderlich, um diese Zuordnung mit der Datenquelle zuzuordnen.

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
 

 

## <a name="associationset-element-csdl"></a>AssociationSet-Element (CSDL)

Die **AssociationSet** -Element in konzeptioneller Schemadefinitionssprache (CSDL) ist ein logischer Container für Zuordnungsinstanzen desselben Typs. Ein Zuordnungssatz stellt eine Definition zum Gruppieren von Zuordnungsinstanzen bereit, sodass sie einer Datenquelle zugeordnet werden können.  

Die **AssociationSet** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element zugelassen)
-   Ende (genau zwei Elemente erforderlich)
-   Anmerkungselemente (0 (null) oder mehrere Elemente zugelassen)

Die **Zuordnung** Attribut gibt den Typ der Zuordnung, die ein Zuordnungssatz enthält. Die Entitätenmengen, die die Enden eines Zuordnungssatzes bilden angegeben sind, genau mit zwei untergeordneten **End** Elemente.

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **AssociationSet** Element.

| Attributname  | Ist erforderlich | Wert                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**        | Ja         | Der Name des Entitätssatzes. Der Wert des der **Namen** Attribut darf nicht den Wert der identisch sein der **Zuordnung** Attribut.                                 |
| **Zuordnung** | Ja         | Der vollqualifizierte Name der Zuordnung, von der der Zuordnungssatz Instanzen enthält. Die Zuordnung muss sich im gleichen Namespace wie der Zuordnungssatz befinden. |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **AssociationSet** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **EntityContainer** -Element mit zwei **AssociationSet** Elemente:

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
 

 

## <a name="collectiontype-element-csdl"></a>CollectionType-Element (CSDL)

Die **CollectionType** -Element in konzeptioneller Schemadefinitionssprache (CSDL) gibt an, dass ein Funktionsparameter oder Funktion Rückgabetyp ist eine Sammlung. Die **CollectionType** Element kann ein untergeordnetes Element des Parameter-Element oder das Element ReturnType (Funktion) sein. Der Typ der Auflistung kann angegeben werden, indem Sie entweder die **Typ** Attribut oder eine der folgenden untergeordneten Elemente:

-   **CollectionType**
-   ReferenceType
-   RowType
-   TypeRef

> [!NOTE]
> Ein Modell überprüft nicht, ob der Typ einer Auflistung mit den beiden angegeben ist die **Typ** -Attribut und einem untergeordneten Element.

 

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **CollectionType** Element. Beachten Sie, dass die **DefaultValue**, **MaxLength**, **FixedLength**, **Genauigkeit**, **Skalierung**,  **Unicode**, und **Sortierreihenfolge** Attribute gelten nur für Auflistungen von **abzielt**.

| Attributname                                                          | Ist erforderlich | Wert                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**                                                                | Nein          | Der Typ der Auflistung.                                                                                                                                                                                                      |
| **NULL-Werte zulässt**                                                            | Nein          | **"True"** (Standardwert) oder **"false"** abhängig davon, ob die Eigenschaft einen null-Wert haben kann. <br/> [!NOTE]                                                                                                                 |
| > In der CSDL-Version 1, eine Eigenschaft eines komplexen Typs benötigen `Nullable="False"`. |             |                                                                                                                                                                                                                                  |
| **DefaultValue**                                                        | Nein          | Der Standardwert der Eigenschaft.                                                                                                                                                                                               |
| **MaxLength**                                                           | Nein          | Die maximale Länge des Eigenschaftswerts.                                                                                                                                                                                        |
| **FixedLength**                                                         | Nein          | **"True"** oder **"false"** abhängig davon, ob der Eigenschaftswert als Zeichenfolge mit fester Länge gespeichert wird.                                                                                                                           |
| **Genauigkeit**                                                           | Nein          | Die Genauigkeit des Eigenschaftswerts.                                                                                                                                                                                             |
| **Scale** (Skalieren)                                                               | Nein          | Die Skalierung des Eigenschaftswerts.                                                                                                                                                                                                 |
| **SRID**                                                                | Nein          | Räumliche Verweis Systembezeichner. Gilt nur für Eigenschaften des räumlichen Typen.   Weitere Informationen finden Sie unter [SRID](http://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx) |
| **Unicode**                                                             | Nein          | **"True"** oder **"false"** abhängig davon, ob der Eigenschaftswert als Unicode-Zeichenfolge gespeichert wird.                                                                                                                                |
| **Sortierung**                                                           | Nein          | Eine Zeichenfolge, die die Sortierreihenfolge angibt, die in der Datenquelle verwendet werden soll.                                                                                                                                                    |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **CollectionType** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine modelldefinierte Funktion, die mithilfe, einer **CollectionType** Element, um anzugeben, dass die Funktion, eine Auflistung von zurückgibt **Person** Entitätstypen (gemäß der Angabe der **ElementType** Attribut).

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
 

Das folgende Beispiel zeigt eine modelldefinierte Funktion, verwendet eine **CollectionType** Element, um anzugeben, dass die Funktion eine Auflistung von Zeilen zurückgibt (gemäß den Angaben in der **RowType** Element).

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
 

Das folgende Beispiel zeigt eine modelldefinierte Funktion, verwendet der **CollectionType** Element, um anzugeben, dass die Funktion als Parameter, eine Auflistung von akzeptiert **Abteilung** Entitätstypen.

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
 

 

## <a name="complextype-element-csdl"></a>ComplexType-Element (CSDL)

Ein **ComplexType** -Element definiert eine Datenstruktur, bestehend aus **EdmSimpleType** Eigenschaften oder anderen komplexen Typen.  Ein komplexer Typ kann eine Eigenschaft eines Entitätstyps oder eines anderen komplexen Typs sein. Ein komplexer Typ entspricht einem Entitätstyp, in dem von einem komplexen Typ Daten definiert werden. Es gibt jedoch einige Hauptunterschiede zwischen komplexen Typen und Entitätstypen:

-   Komplexe Typen weisen keine Identitäten (oder Schlüssel) auf und können daher nicht unabhängig sein. Komplexe Typen können nur Eigenschaften von Entitätstypen oder anderen komplexen Typen sein.
-   Komplexe Typen können nicht an Zuordnungen beteiligt sein. Weder Ende einer Zuordnung kann ein komplexer Typ sein, und daher Navigationseigenschaften können nicht für komplexe Typen definiert werden.
-   Einer komplexen Typeigenschaft kann kein NULL-Wert zugewiesen werden, obwohl jede skalare Eigenschaft eines komplexen Typs auf NULL festgelegt werden kann.

Ein **ComplexType** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element)
-   Eigenschaft (null oder mehr Elemente)
-   Anmerkungselemente (null oder mehr Elemente)

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **ComplexType** Element.

| Attributname                                                                                                 | Ist erforderlich | Wert                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| name                                                                                                           | Ja         | Der Name des komplexen Typs. Der Name eines komplexen Typs darf nicht dem Namen anderer komplexer Typen, Entitätstypen oder Zuordnungen entsprechen, die sich innerhalb des Bereichs des Modells befinden. |
| BaseType                                                                                                       | Nein          | Der Name eines anderen komplexen Typs, der der Basistyp des zu definierenden komplexen Typs ist. <br/> [!NOTE]                                                                     |
| > Dieses Attribut ist nicht anwendbar, in der CSDL-v1. Für komplexe Typen wird Vererbung in dieser Version nicht unterstützt. |             |                                                                                                                                                                                     |
| Abstrakt                                                                                                       | Nein          | **"True"** oder **"false"** (Standardwert) abhängig davon, ob der komplexe Typ ein abstrakter Typ. <br/> [!NOTE]                                                                  |
| > Dieses Attribut ist nicht anwendbar, in der CSDL-v1. Komplexe Typen in dieser Version können keine abstrakten Typen sein.         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **ComplexType** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt einen komplexen Typ **Adresse**, mit der **EdmSimpleType** Eigenschaften **StreetAddress**, **City**,  **StateOrProvince**, **Land**, und **PostalCode**.

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

Um den komplexen Typ definieren **Adresse** (siehe oben) als Eigenschaft eines Entitätstyps handelt, müssen Sie deklarieren den Eigenschaftentyp in der Definition des Entitätstyps. Das folgende Beispiel zeigt die **Adresse** Eigenschaft als komplexen Typ für einen Entitätstyp (**Verleger**):

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
 

 

## <a name="definingexpression-element-csdl"></a>DefiningExpression-Element (CSDL)

Die **DefiningExpression** -Element in konzeptioneller Schemadefinitionssprache (CSDL) enthält einen Entity SQL-Ausdruck, der eine Funktion im konzeptionellen Modell definiert.  

> [!NOTE]
> Zu Validierungszwecken kann ein **DefiningExpression** -Element beliebigen Inhalt enthalten. Allerdings Entity Framework löst eine Ausnahme zur Laufzeit Wenn eine **DefiningExpression** Element enthält keinen gültigen Entity SQL.

 

### <a name="applicable-attributes"></a>Anwendbare Attribute

Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **DefiningExpression** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

### <a name="example"></a>Beispiel

Im folgenden Beispiel wird eine **DefiningExpression** Element eine Funktion definiert, die die Anzahl der Jahre zurückgibt, da ein Buch veröffentlicht wurde. Den Inhalt der **DefiningExpression** Element ist in Entity SQL geschrieben.

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a>Dependent-Element (CSDL)

Die **abhängige** -Element in konzeptioneller Schemadefinitionssprache (CSDL) ist ein untergeordnetes Element auf das ReferentialConstraint-Element und definiert das abhängige Ende einer referenziellen Einschränkung. Ein **ReferentialConstraint** -Element definiert Funktionen, die eine Einschränkung der referenziellen Integrität in einer relationalen Datenbank ähnlich ist. So wie eine Spalte (oder Spalten) einer Datenbanktabelle auf den Primärschlüssel einer anderen Tabelle verweisen kann, kann eine Eigenschaft (oder Eigenschaften) eines Entitätstyps auf den Entitätsschlüssel eines anderen Entitätstyps verweisen. Wird aufgerufen, der Entitätstyp, auf die verwiesen wird, wird die *prinzipalende* der Einschränkung. Der Entitätstyp, der auf das prinzipalende verweist heißt die *abhängigen Endes* der Einschränkung. **PropertyRef** Elemente werden verwendet, um anzugeben, welche Schlüssel auf das prinzipalende verweisen.

Die **abhängige** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   PropertyRef (eine oder mehrere Elemente)
-   Anmerkungselemente (null oder mehr Elemente)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **abhängige** Element.

| Attributname | Ist erforderlich | Wert                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Rolle**       | Ja         | Der Name des Entitätstyps am abhängigen Ende der Zuordnung. |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **abhängige** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **ReferentialConstraint** Element verwendet wird, als Teil der Definition der **PublishedBy** Zuordnung. Die **PublisherId** Eigenschaft der **Buch** -Entitätstyps bildet das abhängige Ende der referenziellen Einschränkung.

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
 

 

## <a name="documentation-element-csdl"></a>Documentation-Element (CSDL)

Die **Dokumentation** -Element in konzeptioneller Schemadefinitionssprache (CSDL) kann verwendet werden, um Informationen zu einem Objekt bereitzustellen, die in einem übergeordneten Element definiert ist. In einer EDMX-Datei Wenn die **Dokumentation** Element ist ein untergeordnetes Element eines Elements, das als ein Objekt auf der Entwurfsoberfläche des EF-Designers (z. B. eine Entität, Zuordnung oder Eigenschaft), den Inhalt der angezeigt wird der **Dokumentation**  Element wird angezeigt, in der Visual Studio **Eigenschaften** Fenster für das Objekt.

Die **Dokumentation** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   **Zusammenfassung**: eine kurze Beschreibung des übergeordneten Elements. (kein (Null) oder ein Element)
-   **LongDescription**: eine ausführliche Beschreibung des übergeordneten Elements. (kein (Null) oder ein Element)
-   Anmerkungselemente. (keine (Null) oder mehrere Elemente)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Dokumentation** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt die **Dokumentation** Element als untergeordnetes Element eines EntityType-Elements. Wäre im Codeausschnitt weiter unten in der CSDL Inhalt einer EDMX-Datei, den Inhalt der **Zusammenfassung** und **LongDescription** Elemente angezeigt werden würden, in der Visual Studio **Eigenschaften** Fenster beim Klicken auf die `Customer` Entitätstyp.

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
 

 

## <a name="end-element-csdl"></a>End-Element (CSDL)

Die **End** -Element in konzeptioneller Schemadefinitionssprache (CSDL) kann ein untergeordnetes Element des Association-Element oder das AssociationSet-Element sein. In jedem Fall die Rolle der **End** Elements und die anwendbaren Attribute unterscheiden.

### <a name="end-element-as-a-child-of-the-association-element"></a>Das End-Element als untergeordnetes Objekt des Association-Elements

Ein **End** Element (als untergeordnetes Element der **Zuordnung** Element) gibt den Entitätstyp auf ein Ende einer Zuordnung und die Anzahl der Instanzen eines Entitätstyps, die an diesem Ende einer Zuordnung vorhanden sein können. Zuordnungsenden werden als Teil einer Zuordnung definiert. Eine Zuordnung muss genau zwei Zuordnungsenden aufweisen. Auf Entitätstypinstanzen an einem Ende einer Zuordnung kann über Navigationseigenschaften oder Fremdschlüssel zugegriffen werden, sofern sie für einen Entitätstyp verfügbar gemacht werden.  

Ein **End** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element)
-   OnDelete (0 (null) oder ein Element)
-   Anmerkungselemente (null oder mehr Elemente)

#### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **End** -Element, wenn es sich um das untergeordnete Element ist ein **Zuordnung** Element.

| Attributname   | Ist erforderlich | Wert                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**         | Ja         | Der Name des Entitätstyps an einem Ende der Zuordnung.                                                                                                                                                                                                                                                                                                                                                         |
| **Rolle**         | Nein          | Der Name für das Zuordnungsende. Wird kein Name angegeben, wird der Name des Entitätstyps am Zuordnungsende verwendet.                                                                                                                                                                                                                                                                                           |
| **Multiplizität** | Ja         | **1**, **0.. 1**, oder **\*** abhängig von der Anzahl der Entitätstypinstanzen, die am Ende der Zuordnung sein können. <br/> **1** gibt an, genau eine Entitätstypinstanz am Zuordnungsende vorhanden. <br/> **0.. 1** gibt an, dass 0 (null) oder eine Entitätstypinstanz am Zuordnungsende vorhanden ist. <br/> **\*** Gibt an, dass 0 (null), einer oder mehrere Instanzen eines Entitätstyps am Zuordnungsende vorhanden sind. |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **End** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

#### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **Zuordnung** -Element, definiert der **CustomerOrders** Zuordnung. Die **Multiplizität** Werte für die einzelnen **End** der Zuordnung angegeben, dass viele **Bestellungen** zugeordnet sein kann eine **Kunden**, aber nur ein **Kunden** zugeordnet sein kann ein **Reihenfolge**. Darüber hinaus die **OnDelete** Element gibt an, dass alle **Bestellungen** , beziehen sich auf eine bestimmte **Kunden** und in geladen wurden ObjectContext werden Gelöschte If der **Kunden** wird gelöscht.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a>Das End-Element als untergeordnetes Objekt des AssociationSet-Elements

Die **End** -Element gibt ein Ende des Zuordnungssatzes. Die **AssociationSet** -Element muss enthalten zwei **End** Elemente. Die Informationen in einer **End** Element wird beim Zuordnen eines Zuordnungssatzes zu einer Datenquelle verwendet.

Ein **End** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element)
-   Anmerkungselemente (null oder mehr Elemente)

> [!NOTE]
> Anmerkungselemente müssen an alle anderen untergeordneten Elemente angereiht werden. Anmerkungselemente sind nur zulässig, in der CSDL-v2 und höher.

 

#### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **End** -Element, wenn es sich um das untergeordnete Element ist ein **AssociationSet** Element.

| Attributname | Ist erforderlich | Wert                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **EntitySet**  | Ja         | Der Name des der **EntitySet** Element, das ein Ende des übergeordneten Elements definiert **AssociationSet** Element. Die **EntitySet** Element definiert werden muss, in der gleichen Entitätscontainer wie das übergeordnete Element **AssociationSet** Element. |
| **Rolle**       | Nein          | Der Name des Endes des Zuordnungssatzes. Wenn die **Rolle** Attribut nicht verwendet wird, der Name des zuordnungssatzendes wird der Name der Entitätenmenge sein.                                                                   |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **End** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

#### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **EntityContainer** -Element mit zwei **AssociationSet** Elementen, die jeweils mit zwei **End** Elemente:

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
 

 

## <a name="entitycontainer-element-csdl"></a>EntityContainer-Element (CSDL)

Die **EntityContainer** -Element in konzeptioneller Schemadefinitionssprache (CSDL) ist ein logischer Container für Entitätssätze, Zuordnungssätze und Funktionsimporte. Ordnet ein Entitätscontainer eines konzeptionellen Modells ein Speichermodell-Entitätscontainer über die EntityContainerMapping-Element. Ein Speichermodell-Entitätscontainer beschreibt die Struktur der Datenbank: Entitätssätze beschreiben Tabellen, Zuordnungssätze beschreiben Fremdschlüsseleinschränkungen, und Funktionsimporte beschreiben gespeicherte Prozeduren in einer Datenbank.

Ein **EntityContainer** -Element kann NULL oder eine dokumentationselemente aufweisen. Wenn eine **Dokumentation** -Element vorhanden ist, muss sich davor, alle **EntitySet**, **AssociationSet**, und **FunctionImport** Elemente.

Ein **EntityContainer** -Element kann 0 (null) oder mehrere der folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet) aufweisen:

-   EntitySet
-   AssociationSet
-   FunctionImport
-   Anmerkungselemente

Sie können erweitern, eine **EntityContainer** zum Angeben von den Inhalt eines anderen **EntityContainer** , der sich im selben Namespace. Um den Inhalt eines anderen einzuschließen **EntityContainer**, im verweisenden **EntityContainer** -Element legen Sie den Wert des der **erweitert** -Attribut auf den Namen der  **EntityContainer** -Element, das enthalten sein sollen. Alle untergeordneten Elemente des eingeschlossenen **EntityContainer** Element behandelt werden als untergeordnete Elemente des verweisenden **EntityContainer** Element.

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Using** Element.

| Attributname | Ist erforderlich | Wert                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Name**       | Ja         | Der Name des Entitätscontainers.                               |
| **Erweitert**    | Nein          | Der Name eines anderen Entitätscontainers innerhalb des gleichen Namespaces. |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **EntityContainer** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **EntityContainer** -Element, das drei Entitätenmengen und zwei Zuordnungssätze definiert.

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
 

 

## <a name="entityset-element-csdl"></a>EntitySet-Element (CSDL)

Die **EntitySet** -Element in konzeptioneller Schemadefinitionssprache ist ein logischer Container für Instanzen eines Entitätstyps und Instanzen eines beliebigen Typs, die von diesem Entitätstyp abgeleitet ist. Die Beziehung zwischen einem Entitätstyp und einem Entitätssatz ist zur Beziehung zwischen einer Zeile und einer Tabelle in einer relationalen Datenbank analog. Wie eine Zeile definiert ein Entitätstyp einen Satz verknüpfter Daten, und ebenso wie eine Tabelle enthält ein Entitätssatz Instanzen dieser Definition. Ein Entitätssatz stellt ein Konstrukt zum Gruppieren von Entitätstypinstanzen bereit, damit diese verwandten Datenstrukturen in einer Datenquelle zugeordnet werden können.  

Für einen bestimmten Entitätstyp kann mindestens ein Entitätssatz definiert werden.

> [!NOTE]
> Dem EF Designer unterstützt keine konzeptionelle Modellen, die mehrere Entitätenmengen pro Typ enthalten.

 

Die **EntitySet** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Documentation-Element (null oder ein Element zugelassen)
-   Anmerkungselemente (0 (null) oder mehrere Elemente zugelassen)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **EntitySet** Element.

| Attributname | Ist erforderlich | Wert                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Name**       | Ja         | Der Name des Entitätssatzes.                                                              |
| **EntityType** | Ja         | Der vollqualifizierte Name des Entitätstyps, für den der Entitätssatz Instanzen enthält. |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **EntitySet** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **EntityContainer** -Element mit drei **EntitySet** Elemente:

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
 

Pro Typ (MEST) können mehrere Entitätssätze definiert werden. Das folgende Beispiel definiert einen Entitätscontainer mit zwei Entitätenmengen für den **Buch** Entitätstyp:

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
 

 

## <a name="entitytype-element-csdl"></a>EntityType-Element (CSDL)

Die **EntityType** -Element stellt die Struktur des Konzept der obersten Ebene, z. B. ein Kunde oder eine Bestellung in einem konzeptionellen Modell dar. Ein Entitätstyp ist eine Vorlage für Instanzen von Entitätstypen in einer Anwendung. Jede Vorlage enthält die folgenden Informationen:

-   Eine eindeutige Bezeichnung. (Erforderlich.)
-   Ein Entitätsschlüssel, der von einem oder mehreren Eigenschaften definiert wird. (Erforderlich.)
-   Eigenschaften für enthaltene Daten. (Optional)
-   Navigationseigenschaften, die die Navigation von einem Ende einer Zuordnung zum anderen Ende ermöglichen. (Optional)

In einer Anwendung stellt eine Instanz eines Entitätstyps ein spezielles Objekt dar, wie etwa einen bestimmten Kunden oder eine Bestellung. Jede Instanz eines Entitätstyps muss über einen eindeutigen Entitätsschlüssel innerhalb eines Entitätssatzes verfügen.

Zwei Instanzen eines Entitätstyps werden nur dann als gleich betrachtet, wenn sie vom selben Typ sind und die Werte ihrer Entitätsschlüssel übereinstimmen.

Ein **EntityType** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element)
-   Schlüssel (0 (null) oder ein Element)
-   Eigenschaft (null oder mehr Elemente)
-   NavigationProperty (null oder mehr Elemente)
-   Anmerkungselemente (null oder mehr Elemente)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **EntityType** Element.

| Attributname                                                                                                                                  | Ist erforderlich | Wert                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Name**                                                                                                                                        | Ja         | Der Name des Entitätstyps.                                                                     |
| **BaseType**                                                                                                                                    | Nein          | Der Name eines anderen Entitätstyps, der der Basistyp des zu definierenden Entitätstyps ist.  |
| **Abstrakt**                                                                                                                                    | Nein          | **"True"** oder **"false"**, abhängig davon, ob der Entitätstyp ein abstrakter Typ ist.                 |
| **OpenType**                                                                                                                                    | Nein          | **"True"** oder **"false"** abhängig davon, ob der Entitätstyp ein offener Entitätstyp ist. <br/> [!NOTE] |
| > Die **OpenType** Attribut gilt nur für Entitätstypen, die in konzeptionellen Modellen definiert werden, die mit ADO.NET Data Services verwendet werden. |             |                                                                                                  |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **EntityType** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **EntityType** -Element mit drei **Eigenschaft** Elementen und zwei **NavigationProperty** Elemente:

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
 

 

## <a name="enumtype-element-csdl"></a>EnumType-Element (CSDL)

Die **EnumType** Element stellt einen Enumerationstyp dar.

Ein **EnumType** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element)
-   Element (null oder mehr Elemente)
-   Anmerkungselemente (null oder mehr Elemente)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **EnumType** Element.

| Attributname     | Ist erforderlich | Wert                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**           | Ja         | Der Name des Entitätstyps.                                                                                                                                                                  |
| **IsFlags**        | Nein          | **"True"** oder **"false"**, abhängig davon, ob der Enumerationstyp als einen Satz von Flags verwendet werden kann. Der Standardwert ist **"false".**.                                                                     |
| **UnderlyingType** | Nein          | **Edm.Byte**, **Edm.Int16**, **Int32**, **Int64** oder **Edm.SByte** definieren den Bereich von Werten des Typs.   Die zugrunde liegende Standardtyp von Enumerationselementen ist **Int32.**. |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **EnumType** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **EnumType** -Element mit drei **Member** Elemente:

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a>Function-Element (CSDL)

Die **Funktion** -Element in konzeptioneller Schemadefinitionssprache (CSDL) wird verwendet, um zu definieren oder Deklarieren von Funktionen im konzeptionellen Modell. Eine Funktion wird mit einem DefiningExpression-Element definiert.  

Ein **Funktion** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element)
-   Parameter (null oder mehr Elemente)
-   DefiningExpression (0 (null) oder ein Element)
-   ReturnType (Funktion) (0 (null) oder ein Element)
-   Anmerkungselemente (null oder mehr Elemente)

Ein Rückgabewert für eine Funktion muss angegeben werden, entweder mit der **ReturnType** (Funktion) Element oder die **ReturnType** -Attribut (siehe unten), aber nicht beides. Die möglichen Rückgabetypen sind alle EdmSimpleType-Typen, Entitätstypen, komplexe Typen, Zeilentypen (oder eine Auflistung eines dieser Typen) oder Ref-Typen.

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Funktion** Element.

| Attributname | Ist erforderlich | Wert                              |
|:---------------|:------------|:-----------------------------------|
| **Name**       | Ja         | Der Name der Funktion.          |
| **ReturnType** | Nein          | Der von der Funktion zurückgegebene Typ. |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Funktion** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

### <a name="example"></a>Beispiel

Im folgenden Beispiel wird eine **Funktion** Element eine Funktion definiert, die die Anzahl der Jahre zurückgibt, da die Einstellung einer Lehrkraft vergangen.

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a>FunctionImport-Element (CSDL)

Die **FunctionImport** -Element in konzeptioneller Schemadefinitionssprache (CSDL) stellt eine Funktion, die in der Datenquelle definiert, aber für andere Objekte über das konzeptionelle Modell verfügbar ist. Beispielsweise kann im Speichermodell ein Function-Element verwendet werden, um eine gespeicherte Prozedur in einer Datenbank darzustellen. Ein **FunctionImport** -Element im konzeptionellen Modell stellt die entsprechende Funktion in einer Entity Framework-Anwendung und der speichermodellfunktion mithilfe des FunctionImportMapping-Elements zugeordnet ist. Wird die Funktion in der Anwendung aufgerufen, wird die entsprechende gespeicherte Prozedur in der Datenbank ausgeführt.

Die **FunctionImport** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element zugelassen)
-   Parameter (0 (null) oder mehrere Elemente zugelassen)
-   Anmerkungselemente (0 (null) oder mehrere Elemente zugelassen)
-   ReturnType (FunctionImport) (0 (null) oder mehrere Elemente zugelassen)

Eine **Parameter** Element sollte für jeden von der Funktion akzeptierten Parameter definiert werden.

Ein Rückgabewert für eine Funktion muss angegeben werden, entweder mit der **ReturnType** (FunctionImport)-Element oder die **ReturnType** -Attribut (siehe unten), aber nicht beides. Der Rückgabetyp-Wert muss es sich um eine Auflistung von ComplexType, EntityType oder EdmSimpleType sein.

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **FunctionImport** Element.

| Attributname   | Ist erforderlich | Wert                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**         | Ja         | Der Name der importierten Funktion.                                                                                                                                                                    |
| **ReturnType**   | Nein          | Der Typ, den die Funktion zurückgibt. Verwenden Sie dieses Attribut nicht, wenn die Funktion keinen Wert zurückgibt. Andernfalls muss der Wert eine Auflistung von ComplexType, EntityType oder EDMSimpleType sein.        |
| **EntitySet**    | Nein          | Wenn die Funktion eine Auflistung von Entitätstypen zurückgibt Typen verwenden, wird der Wert von der **EntitySet** muss der Entitätenmenge entsprechen, zu dem die Auflistung gehört. Andernfalls die **EntitySet** Attribut darf nicht verwendet werden. |
| **IsComposable** | Nein          | Wenn der Wert festgelegt ist, auf "true", die Funktion ist effizienter, zusammensetzbarer (Table-valued Function) und kann in einer LINQ-Abfrage verwendet werden.  Der Standardwert ist **FALSE**.                                                           |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **FunctionImport** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **FunctionImport** Element, das einen Parameter akzeptiert und eine Auflistung von Entitätstypen zurückgibt:

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a>Key-Element (CSDL)

Die **Schlüssel** Element ist ein untergeordnetes Element des EntityType-Elements und definiert eine *Entitätsschlüssel* (eine Eigenschaft oder einen Satz von Eigenschaften eines Entitätstyps, der Identität bestimmt). Die Eigenschaften, die einen Entitätsschlüssel bilden, werden zur Entwurfszeit ausgewählt. Die Werte von Entitätsschlüsseleigenschaften müssen zur Laufzeit eindeutig eine Entitätstypinstanz innerhalb eines Entitätssatzes identifizieren. Die Eigenschaften, die einen Entitätsschlüssel bilden, sollten so ausgewählt werden, dass die Eindeutigkeit von Instanzen in einem Entitätssatz gewährleistet ist. Die **Schlüssel** Element definiert einen Entitätsschlüssel durch Verweisen auf eine oder mehrere der Eigenschaften eines Entitätstyps handelt.

Die **Schlüssel** -Element kann die folgenden untergeordneten Elemente aufweisen:

-   PropertyRef (eine oder mehrere Elemente)
-   Anmerkungselemente (null oder mehr Elemente)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Schlüssel** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

### <a name="example"></a>Beispiel

Das folgende Beispiel definiert einen Entitätstyp, der mit dem Namen **Buch**. Der Entitätsschlüssel wird definiert durch Verweisen auf die **ISBN** Eigenschaft des Entitätstyps.

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
 

Die **ISBN** Eigenschaft ist eine gute Wahl für den Entitätsschlüssel aus, da ein internationaler Standard Buch Anzahl (ISBN) ein Buch eindeutig identifiziert.

Das folgende Beispiel zeigt einen Entitätstyp (**Autor**), die einen Entitätsschlüssel, der aus zwei Eigenschaften besteht, hat **Namen** und **Adresse**.

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
 

Mithilfe von **Namen** und **Adresse** für die Entität ist empfehlenswert, da zwei Autoren mit demselben Namen wahrscheinlich nicht an derselben Adresse live sind. Dieser Entitätsschlüssel garantiert jedoch nicht absolut eindeutige Entitätsschlüssel in einem Entitätssatz. Hinzufügen einer Eigenschaft, z. B. **AutorID**, die verwendet werden kann, zur eindeutigen Identifizierung ein Autors empfiehlt sich in diesem Fall.

 

## <a name="member-element-csdl"></a>Member-Element (CSDL)

Die **Member** Element ist ein untergeordnetes Element des Elements EnumType und ein Mitglied über den enumerierten Typ definiert.

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **FunctionImport** Element.

| Attributname | Ist erforderlich | Wert                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**       | Ja         | Der Name des Members.                                                                                                                                                                  |
| **Wert**      | Nein          | Der Wert des Members. Wird standardmäßig das erste Element hat den Wert 0, und der Wert jedes nachfolgenden Enumerators wird um 1 erhöht. Mehrere Elemente mit den gleichen Werten können vorhanden sein. |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **FunctionImport** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **EnumType** -Element mit drei **Member** Elemente:

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a>NavigationProperty-Element (CSDL)

Ein **NavigationProperty** -Element definiert eine Navigationseigenschaft, die einen Verweis auf das andere Ende einer Zuordnung bereitstellt. Im Gegensatz zu Eigenschaften, die mit dem Property-Element definiert werden werden Navigationseigenschaften nicht die Form und die Eigenschaften der Daten definieren. Sie bieten eine Möglichkeit, eine Zuordnung zwischen zwei Entitätstypen zu navigieren.

Beachten Sie, dass Navigationseigenschaften für beide Entitätstypen an den Enden einer Zuordnung optional sind. Wenn Sie für einen Entitätstyp am Ende einer Zuordnung eine Navigationseigenschaft definieren, muss keine Navigationseigenschaft für den Entitätstyp am anderen Ende der Zuordnung definiert werden.

Der von einer Navigationseigenschaft zurückgegebene Datentyp wird von der Multiplizität des Remotezuordnungsendes bestimmt. Nehmen wir beispielsweise an eine Navigationseigenschaft, **OrdersNavProp**, vorhanden ist, auf eine **Kunden** Entitätstyp und navigiert eine 1: n Zuordnung zwischen **Kunden** und  **Reihenfolge**. Da das remotezuordnungsende für die Navigationseigenschaft eine Multiplizität viele aufweist (\*), der Datentyp ist eine Sammlung (der **Reihenfolge**). Auf ähnliche Weise, wenn eine Navigationseigenschaft, **CustomerNavProp**, vorhanden ist, auf die **Reihenfolge** Entitätstyp, dessen Datentyp wäre **Kunden** , da die Multiplizität des remoteendes ist eins (1).

Ein **NavigationProperty** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element)
-   Anmerkungselemente (null oder mehr Elemente)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **NavigationProperty** Element.

| Attributname   | Ist erforderlich | Wert                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**         | Ja         | Der Name der Navigationseigenschaft.                                                                                                                                                                                                             |
| **Beziehung** | Ja         | Der Name einer Zuordnung, die sich innerhalb des Bereichs des Modells befindet.                                                                                                                                                                                |
| **ToRole**       | Ja         | Das Ende der Zuordnung, an dem die Navigation endet. Der Wert des der **ToRole** Attribut muss identisch mit dem Wert von einem der der **Rolle** Attribute, die für einen der Zuordnungsenden (definiert im AssociationEnd-Element) definiert.       |
| **FromRole**     | Ja         | Das Ende der Zuordnung, an dem die Navigation beginnt. Der Wert des der **FromRole** Attribut muss identisch mit dem Wert von einem der der **Rolle** Attribute, die für einen der Zuordnungsenden (definiert im AssociationEnd-Element) definiert. |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **NavigationProperty** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

### <a name="example"></a>Beispiel

Das folgende Beispiel definiert einen Entitätstyp (**Buch**) mit zwei Navigationseigenschaften (**PublishedBy** und **WrittenBy**):

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
 

 

## <a name="ondelete-element-csdl"></a>OnDelete-Element (CSDL)

Die **OnDelete** -Element in konzeptioneller Schemadefinitionssprache (CSDL) definiert, Verhalten, das mit einer Zuordnung verbunden ist. Wenn die **Aktion** -Attributsatz auf **Cascade** an einem Ende einer Zuordnung im Zusammenhang Entitätstypen am anderen Ende der Zuordnung werden gelöscht, wenn der Entitätstyp am ersten Ende gelöscht wird. Wenn die Zuordnung zwischen zwei Entitätstypen eine Primärschlüssel-zu-Primärschlüssel-Beziehung, wird ein geladenes abhängiges Objekt gelöscht, wenn das Prinzipalobjekt am anderen Ende der Zuordnung, unabhängig von gelöscht wird der **OnDelete** Spezifikation.  

> [!NOTE]
> Die **OnDelete** Element wirkt sich nur auf das Laufzeitverhalten einer Anwendung, die sie nicht auf das Verhalten in der Datenquelle. Das in der Datenquelle definierte Verhalten sollte dem in der Anwendung definierten Verhalten entsprechen.

 

Ein **OnDelete** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element)
-   Anmerkungselemente (null oder mehr Elemente)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **OnDelete** Element.

| Attributname | Ist erforderlich | Wert                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Aktion**     | Ja         | **CASCADE** oder **keine**. Wenn **Cascade**, abhängige Entitätstypen gelöscht, sobald der prinzipalentitätstyp gelöscht wird. Wenn **keine**, abhängige Entitätstypen nicht gelöscht, sobald der prinzipalentitätstyp gelöscht wird. |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Zuordnung** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **Zuordnung** -Element, definiert der **CustomerOrders** Zuordnung. Die **OnDelete** Element gibt an, dass alle **Bestellungen** , beziehen sich auf eine bestimmte **Kunden** und in den ObjectContext geladen wurden gelöscht werden, sobald die  **Kunden** wird gelöscht.

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a>Parameter-Element (CSDL)

Die **Parameter** -Element in konzeptioneller Schemadefinitionssprache (CSDL) kann ein untergeordnetes Element des FunctionImport-Elements oder Function-Element sein.

### <a name="functionimport-element-application"></a>FunctionImport-Element-Anwendung

Ein **Parameter** -Element (als untergeordnetes Element der **FunctionImport** Element) wird verwendet, um die Parameter von Eingabe- und Ausgabeparameter für Funktionsimporte, die deklariert werden in CSDL definiert.

Die **Parameter** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element zugelassen)
-   Anmerkungselemente (0 (null) oder mehrere Elemente zugelassen)

#### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Parameter** Element.

| Attributname | Ist erforderlich | Wert                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**       | Ja         | Der Name des Parameters.                                                                                                                                                                                                      |
| **Type**       | Ja         | Der Typ des Parameters. Der Wert muss eine **EDMSimpleType** oder ein komplexer Typ, der innerhalb des Bereichs des Modells ist.                                                                                                             |
| **Modus**       | Nein          | **In**, **Out**, oder **InOut** abhängig davon, ob der Parameter eine Eingabe, Ausgabe oder Eingabe-/Ausgabeparameter.                                                                                                                |
| **MaxLength**  | Nein          | Die maximal zulässige Länge des Parameters.                                                                                                                                                                                    |
| **Genauigkeit**  | Nein          | Die Genauigkeit des Parameters.                                                                                                                                                                                                 |
| **Scale** (Skalieren)      | Nein          | Die Skalierung des Parameters.                                                                                                                                                                                                     |
| **SRID**       | Nein          | Räumliche Verweis Systembezeichner. Gilt nur für Parameter des räumlichen Typen. Weitere Informationen finden Sie unter [SRID](http://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Parameter** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

#### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **FunctionImport** -Element mit einem **Parameter** untergeordnetes Element. Die Funktion akzeptiert einen Eingabeparameter und gibt eine Auflistung von Entitätstypen zurück.

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a>Function-Element-Anwendung

Ein **Parameter** -Element (als untergeordnetes Element der **Funktion** Element) definiert Parameter für Funktionen, die definiert oder deklariert werden in einem konzeptionellen Modell.

Die **Parameter** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 oder 1-Elemente)
-   CollectionType (0 oder 1-Elemente)
-   ReferenceType (0 oder 1-Elemente)
-   RowType (0 oder 1-Elemente)

> [!NOTE]
> Nur eines der der **CollectionType**, **ReferenceType**, oder **RowType** Elemente können ein untergeordnetes Element des werden eine **Eigenschaft** Element.

 

-   Anmerkungselemente (0 (null) oder mehrere Elemente zugelassen)

> [!NOTE]
> Anmerkungselemente müssen an alle anderen untergeordneten Elemente angereiht werden. Anmerkungselemente sind nur zulässig, in der CSDL-v2 und höher.

 

#### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Parameter** Element.

| Attributname   | Ist erforderlich | Wert                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**         | Ja         | Der Name des Parameters.                                                                                                                                                                                                      |
| **Type**         | Nein          | Der Typ des Parameters. Ein Parameter kann einer der folgenden Typen (oder Auflistungen dieser Typen) sein: <br/> **EdmSimpleType** <br/> Entitätstyp <br/> Komplexer Typ <br/> Zeilentyp <br/> Verweistyp                             |
| **NULL-Werte zulässt**     | Nein          | **"True"** (Standardwert) oder **"false"** abhängig davon, ob die Eigenschaft kann einen **null** Wert.                                                                                                                          |
| **DefaultValue** | Nein          | Der Standardwert der Eigenschaft.                                                                                                                                                                                              |
| **MaxLength**    | Nein          | Die maximale Länge des Eigenschaftswerts.                                                                                                                                                                                       |
| **FixedLength**  | Nein          | **"True"** oder **"false"** abhängig davon, ob der Eigenschaftswert als Zeichenfolge mit fester Länge gespeichert wird.                                                                                                                          |
| **Genauigkeit**    | Nein          | Die Genauigkeit des Eigenschaftswerts.                                                                                                                                                                                            |
| **Scale** (Skalieren)        | Nein          | Die Skalierung des Eigenschaftswerts.                                                                                                                                                                                                |
| **SRID**         | Nein          | Räumliche Verweis Systembezeichner. Gilt nur für Eigenschaften des räumlichen Typen. Weitere Informationen finden Sie unter [SRID](http://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**      | Nein          | **"True"** oder **"false"** abhängig davon, ob der Eigenschaftswert als Unicode-Zeichenfolge gespeichert wird.                                                                                                                               |
| **Sortierung**    | Nein          | Eine Zeichenfolge, die die Sortierreihenfolge angibt, die in der Datenquelle verwendet werden soll.                                                                                                                                                   |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Parameter** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

#### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **Funktion** -Element, das eine verwendet **Parameter** untergeordneten-Element, um einen Funktionsparameter zu definieren.

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a>Principal-Element (CSDL)

Die **Principal** -Element in konzeptioneller Schemadefinitionssprache (CSDL) ist ein untergeordnetes Element mit dem ReferentialConstraint-Element, das prinzipalende einer referenziellen Einschränkung definiert. Ein **ReferentialConstraint** -Element definiert Funktionen, die eine Einschränkung der referenziellen Integrität in einer relationalen Datenbank ähnlich ist. So wie eine Spalte (oder Spalten) einer Datenbanktabelle auf den Primärschlüssel einer anderen Tabelle verweisen kann, kann eine Eigenschaft (oder Eigenschaften) eines Entitätstyps auf den Entitätsschlüssel eines anderen Entitätstyps verweisen. Wird aufgerufen, der Entitätstyp, auf die verwiesen wird, wird die *prinzipalende* der Einschränkung. Der Entitätstyp, der auf das prinzipalende verweist heißt die *abhängigen Endes* der Einschränkung. **PropertyRef** Elemente werden verwendet, um anzugeben, welche der Tasten durch das abhängige Ende verwiesen werden.

Die **Principal** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   PropertyRef (eine oder mehrere Elemente)
-   Anmerkungselemente (null oder mehr Elemente)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Principal** Element.

| Attributname | Ist erforderlich | Wert                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| **Rolle**       | Ja         | Der Name des Entitätstyps am Prinzipalende der Zuordnung. |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Principal** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **ReferentialConstraint** -Element, das Teil der Definition ist die **PublishedBy** Zuordnung. Die **Id** Eigenschaft der **Verleger** -Entitätstyps bildet das prinzipalende der referenziellen Einschränkung.

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
 

 

## <a name="property-element-csdl"></a>Property-Element (CSDL)

Die **Eigenschaft** -Element in konzeptioneller Schemadefinitionssprache (CSDL) kann ein untergeordnetes Element des EntityType-Element, das ComplexType-Element oder das RowType-Element sein.

### <a name="entitytype-and-complextype-element-applications"></a>EntityType- und ComplexType-Element-Anwendungen

**Eigenschaft** Elemente (als untergeordnete Elemente des **EntityType** oder **ComplexType** Elemente) definieren die Form und die Merkmale der Daten, die eine Entitätstypinstanz oder komplexe Typinstanz enthält . Eigenschaften in einem konzeptionellen Modell sind analog zu den Eigenschaften, die für eine Klasse definiert werden. So wie Eigenschaften die Form einer Klasse definieren und Informationen zu Objekten enthalten definieren Eigenschaften in einem konzeptionellen Modell die Form eines Entitätstyps und enthalten Informationen zu Entitätstypinstanzen.

Die **Eigenschaft** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Documentation-Element (null oder ein Element zugelassen)
-   Anmerkungselemente (0 (null) oder mehrere Elemente zugelassen)

Die folgenden Facets können angewendet werden, um eine **Eigenschaft** Element: **Nullable**, **DefaultValue**, **MaxLength**,  **FixedLength**, **Genauigkeit**, **Skalierung**, **Unicode**, **Sortierreihenfolge**,  **ConcurrencyMode**. Facets sind XML-Attribute, die Informationen über die Speicherung von Eigenschaftswerten im Datenspeicher bereitstellen.

> [!NOTE]
> Facets können nur auf Eigenschaften des Typs angewendet werden **EDMSimpleType**.

 

#### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Eigenschaft** Element.

| Attributname                                                         | Ist erforderlich | Wert                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                                                               | Ja         | Den Namen der Eigenschaft.                                                                                                                                                                                                       |
| **Type**                                                               | Ja         | Der Typ des Eigenschaftswerts. Der Eigenschaftswerttyp muss ein **EDMSimpleType** oder ein komplexer Typ (durch einen vollqualifizierten Namen angegeben), der im Gültigkeitsbereich des Modells liegt.                                                 |
| **NULL-Werte zulässt**                                                           | Nein          | **"True"** (Standardwert) oder <strong>"false"</strong> abhängig davon, ob die Eigenschaft einen null-Wert haben kann. <br/> [!NOTE]                                                                                                   |
| > In der CSDL-Version 1 Eigenschaft eines komplexen Typs benötigen `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                       | Nein          | Der Standardwert der Eigenschaft.                                                                                                                                                                                              |
| **MaxLength**                                                          | Nein          | Die maximale Länge des Eigenschaftswerts.                                                                                                                                                                                       |
| **FixedLength**                                                        | Nein          | **"True"** oder **"false"** abhängig davon, ob der Eigenschaftswert als Zeichenfolge mit fester Länge gespeichert wird.                                                                                                                          |
| **Genauigkeit**                                                          | Nein          | Die Genauigkeit des Eigenschaftswerts.                                                                                                                                                                                            |
| **Scale** (Skalieren)                                                              | Nein          | Die Skalierung des Eigenschaftswerts.                                                                                                                                                                                                |
| **SRID**                                                               | Nein          | Räumliche Verweis Systembezeichner. Gilt nur für Eigenschaften des räumlichen Typen. Weitere Informationen finden Sie unter [SRID](http://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                            | Nein          | **"True"** oder **"false"** abhängig davon, ob der Eigenschaftswert als Unicode-Zeichenfolge gespeichert wird.                                                                                                                               |
| **Sortierung**                                                          | Nein          | Eine Zeichenfolge, die die Sortierreihenfolge angibt, die in der Datenquelle verwendet werden soll.                                                                                                                                                   |
| **ConcurrencyMode**                                                    | Nein          | **Keine** (Standardwert) oder **Fixed**. Wenn der Wert, um festgelegt ist **Fixed**, den Wert der Eigenschaft an Überprüfungen auf vollständige Parallelität verwendet werden.                                                                                  |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Eigenschaft** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

#### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **EntityType** -Element mit drei **Eigenschaft** Elemente:

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
 

Das folgende Beispiel zeigt eine **ComplexType** -Element mit fünf **Eigenschaft** Elemente:

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a>RowType-Element-Anwendung

**Eigenschaft** Elemente (als untergeordnete Elemente von einem **RowType** Elements) definieren die Form und die Eigenschaften der Daten, die, die an oder von einer modelldefinierten Funktion zurückgegeben werden können.  

Die **Eigenschaft** Element kann nur jeweils eines der folgenden untergeordneten Elemente verfügen:

-   CollectionType
-   ReferenceType
-   RowType

Die **Eigenschaft** -Element kann eine beliebige Anzahl Anmerkung Unterelemente aufweisen.

> [!NOTE]
> Anmerkungselemente sind nur zulässig, in der CSDL-v2 und höher.

 

#### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Eigenschaft** Element.

| Attributname                                                     | Ist erforderlich | Wert                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                                                           | Ja         | Den Namen der Eigenschaft.                                                                                                                                                                                                       |
| **Type**                                                           | Ja         | Der Typ des Eigenschaftswerts.                                                                                                                                                                                                 |
| **NULL-Werte zulässt**                                                       | Nein          | **"True"** (Standardwert) oder **"false"** abhängig davon, ob die Eigenschaft einen null-Wert haben kann. <br/> [!NOTE]                                                                                                                |
| > In CSDL v1 Eigenschaft eines komplexen Typs benötigen `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                   | Nein          | Der Standardwert der Eigenschaft.                                                                                                                                                                                              |
| **MaxLength**                                                      | Nein          | Die maximale Länge des Eigenschaftswerts.                                                                                                                                                                                       |
| **FixedLength**                                                    | Nein          | **"True"** oder **"false"** abhängig davon, ob der Eigenschaftswert als Zeichenfolge mit fester Länge gespeichert wird.                                                                                                                          |
| **Genauigkeit**                                                      | Nein          | Die Genauigkeit des Eigenschaftswerts.                                                                                                                                                                                            |
| **Scale** (Skalieren)                                                          | Nein          | Die Skalierung des Eigenschaftswerts.                                                                                                                                                                                                |
| **SRID**                                                           | Nein          | Räumliche Verweis Systembezeichner. Gilt nur für Eigenschaften des räumlichen Typen. Weitere Informationen finden Sie unter [SRID](http://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | Nein          | **"True"** oder **"false"** abhängig davon, ob der Eigenschaftswert als Unicode-Zeichenfolge gespeichert wird.                                                                                                                               |
| **Sortierung**                                                      | Nein          | Eine Zeichenfolge, die die Sortierreihenfolge angibt, die in der Datenquelle verwendet werden soll.                                                                                                                                                   |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Eigenschaft** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

#### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt **Eigenschaft** Elemente verwendet, um die Form des Rückgabetyps einer modelldefinierten Funktion zu definieren.

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
 

 

## <a name="propertyref-element-csdl"></a>PropertyRef-Element (CSDL)

Die **PropertyRef** -Element in konzeptioneller Schemadefinitionssprache (CSDL) verweist auf eine Eigenschaft eines Entitätstyps, um anzugeben, dass die Eigenschaft eine der folgenden Rollen ausführt:

-   Ein Teil des Entitätsschlüssels (eine Eigenschaft oder ein Satz von Eigenschaften eines Entitätstyps, der die Identität bestimmt). Eine oder mehrere **PropertyRef** -Elemente können verwendet werden, um einen Entitätsschlüssel zu definieren.
-   Das abhängige Ende oder Prinzipalende einer referenziellen Einschränkung.

Die **PropertyRef** Element kann nur Anmerkungselemente (null oder mehr) als untergeordnete Elemente verfügen.

> [!NOTE]
> Anmerkungselemente sind nur zulässig, in der CSDL-v2 und höher.

 

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **PropertyRef** Element.

| Attributname | Ist erforderlich | Wert                                |
|:---------------|:------------|:-------------------------------------|
| **Name**       | Ja         | Der Name der referenzierten Eigenschaft. |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **PropertyRef** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

### <a name="example"></a>Beispiel

Das folgende Beispiel definiert einen Entitätstyp (**Buch**). Der Entitätsschlüssel wird definiert durch Verweisen auf die **ISBN** Eigenschaft des Entitätstyps.

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
 

Im nächsten Beispiel zwei **PropertyRef** Elemente werden verwendet, um anzugeben, dass zwei Eigenschaften (**Id** und **PublisherId**) sind das prinzipalende und das abhängige Ende einer referenziellen Einschränkung.

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
 

 

## <a name="referencetype-element-csdl"></a>ReferenceType-Element (CSDL)

Die **ReferenceType** -Element in konzeptioneller Schemadefinitionssprache (CSDL) gibt einen Verweis auf einen Entitätstyp an. Die **ReferenceType** Element kann ein untergeordnetes Element der folgenden Elemente sein:

-   ReturnType (Funktion)
-   Parameter
-   CollectionType

Die **ReferenceType** Element wird verwendet, wenn ein Parameter oder Rückgabetyp für eine Funktion zu definieren.

Ein **ReferenceType** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element)
-   Anmerkungselemente (null oder mehr Elemente)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **ReferenceType** Element.

| Attributname | Ist erforderlich | Wert                                         |
|:---------------|:------------|:----------------------------------------------|
| **Type**       | Ja         | Der Name des Entitätstyps, auf den verwiesen wird. |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **ReferenceType** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt die **ReferenceType** -Element als untergeordnetes Element des eine **Parameter** Element in einer modelldefinierten Funktion, die akzeptiert einen Verweis auf eine **Person** Entität Typ:

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
 

Das folgende Beispiel zeigt die **ReferenceType** -Element als untergeordnetes Element des eine **ReturnType** (Funktion)-Element in einer modelldefinierten Funktion, die einen Verweis auf eine **Person**Entitätstyp:

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
 

 

## <a name="referentialconstraint-element-csdl"></a>ReferentialConstraint-Element (CSDL)

Ein **ReferentialConstraint** -Element in konzeptioneller Schemadefinitionssprache (CSDL) definiert die Funktionalität, die eine Einschränkung der referenziellen Integrität in einer relationalen Datenbank ähnlich ist. So wie eine Spalte (oder Spalten) einer Datenbanktabelle auf den Primärschlüssel einer anderen Tabelle verweisen kann, kann eine Eigenschaft (oder Eigenschaften) eines Entitätstyps auf den Entitätsschlüssel eines anderen Entitätstyps verweisen. Wird aufgerufen, der Entitätstyp, auf die verwiesen wird, wird die *prinzipalende* der Einschränkung. Der Entitätstyp, der auf das prinzipalende verweist heißt die *abhängigen Endes* der Einschränkung.

Wenn ein Fremdschlüssel, die für einen Entitätstyp verfügbar gemacht ist eine Eigenschaft eines anderen Entitätstyps verweist die **ReferentialConstraint** -Element definiert eine Zuordnung zwischen den zwei Entitätstypen. Da die **ReferentialConstraint** Element enthält Informationen wie zwei Entitätstypen verwandt sind, wird keine entsprechende AssociationSetMapping-Element ist in der Mapping-Spezifikationssprache (MSL) erforderlich. Eine Zuordnung zwischen zwei Entitätstypen, denen keine Fremdschlüssel verfügbar gemacht werden müssen, eine entsprechende **AssociationSetMapping** Element, um die Datenquelle Zuordnungsinformationen zuzuordnen.

Wenn ein Fremdschlüssel nicht für einen Entitätstyp verfügbar gemacht wird die **ReferentialConstraint** -Element kann nur eine primary Key-zu-Primärschlüssel-Einschränkung zwischen dem Entitätstyp und einem anderen Entitätstyp definieren.

Ein **ReferentialConstraint** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element)
-   Prinzipal (genau ein Element)
-   Abhängige (genau ein Element)
-   Anmerkungselemente (null oder mehr Elemente)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die **ReferentialConstraint** -Element kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) aufweisen. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **ReferentialConstraint** Element verwendet wird, als Teil der Definition der **PublishedBy** Zuordnung.

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
 

 

## <a name="returntype-function-element-csdl"></a>ReturnType (Funktion)-Element (CSDL)

Die **ReturnType** (Funktion)-Element in konzeptioneller Schemadefinitionssprache (CSDL) gibt an, den Rückgabetyp für eine Funktion, die in ein Function-Element definiert ist. Eine Funktion, der Rückgabetyp kann auch angegeben werden, mit einem **ReturnType** Attribut.

Rückgabetypen können alle **EdmSimpleType**, Entitätstyp, komplexen Typ, Zeilentyp, Ref-Typ oder eine Auflistung von einem dieser Typen.

Der Rückgabetyp einer Funktion kann angegeben werden, entweder mit der **Typ** Attribut der **ReturnType** (Funktion)-Element, oder mit einem der folgenden untergeordneten Elemente:

-   CollectionType
-   ReferenceType
-   RowType

> [!NOTE]
> Ein Modell überprüft nicht, ob es sich bei Angabe eines funktionsrückgabewerts sowohl die **Typ** Attribut der **ReturnType** (Funktion)-Element und einem der untergeordneten Elemente.

 

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **ReturnType** (Funktion)-Element.

| Attributname | Ist erforderlich | Wert                              |
|:---------------|:------------|:-----------------------------------|
| **ReturnType** | Nein          | Der von der Funktion zurückgegebene Typ. |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **ReturnType** (Funktion)-Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

### <a name="example"></a>Beispiel

Im folgenden Beispiel wird eine **Funktion** Element eine Funktion definiert, die die Anzahl der Jahre zurückgibt, ein Buch gedruckt wurde. Beachten Sie, die durch der Rückgabetyp angegeben wird die **Typ** Attribut eine **ReturnType** (Funktion)-Element.

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a>ReturnType (FunctionImport)-Element (CSDL)

Die **ReturnType** (FunctionImport)-Elements in der konzeptionellen Schemadefinitionssprache (CSDL) gibt an, den Rückgabetyp für eine Funktion, die in FunctionImport-Element definiert ist. Eine Funktion, der Rückgabetyp kann auch angegeben werden, mit einem **ReturnType** Attribut.

Rückgabetypen können eine beliebige Auflistung von Entitätstypen, komplexen Typ oder **EdmSimpleType**,

Der Rückgabetyp einer Funktion wird angegeben, mit der **Typ** Attribut der **ReturnType** (FunctionImport)-Element.

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **ReturnType** (FunctionImport)-Element.

| Attributname | Ist erforderlich | Wert                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**       | Nein          | Der Typ, den die Funktion zurückgibt. Der Wert muss es sich um eine Auflistung von ComplexType, EntityType oder EDMSimpleType sein.                                                                                      |
| **EntitySet**  | Nein          | Wenn die Funktion eine Auflistung von Entitätstypen zurückgibt Typen verwenden, wird der Wert von der **EntitySet** muss der Entitätenmenge entsprechen, zu dem die Auflistung gehört. Andernfalls die **EntitySet** Attribut darf nicht verwendet werden. |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **ReturnType** (FunctionImport)-Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

### <a name="example"></a>Beispiel

Im folgenden Beispiel wird eine **FunctionImport** , Bücher und Verleger zurückgibt. Beachten Sie, dass die Funktion, zwei Resultsets aus, und daher zwei zurückgibt **ReturnType** (FunctionImport)-Element angegeben wird.

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a>RowType-Element (CSDL)

Ein **RowType** -Element in konzeptioneller Schemadefinitionssprache (CSDL) definiert eine unbenannte Struktur als Parameter oder Rückgabetyp für eine Funktion im konzeptionellen Modell definiert.

Ein **RowType** Element kann das untergeordnete Element in der folgenden Elemente sein:

-   CollectionType
-   Parameter
-   ReturnType (Funktion)

Ein **RowType** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Eigenschaft (ein oder mehrere)
-   Anmerkungselemente (null oder mehr)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **RowType** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine modelldefinierte Funktion, verwendet eine **CollectionType** Element, um anzugeben, dass die Funktion eine Auflistung von Zeilen zurückgibt (gemäß den Angaben in der **RowType** Element).

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

## <a name="schema-element-csdl"></a>Schema-Element (CSDL)

Die **Schema** Element ist das Stammelement einer konzeptionellen Modelldefinition. Es enthält Definitionen für die Objekte, Funktionen und Container, die ein konzeptionelles Modell bilden.

Die **Schema** -Element kann 0 (null) oder mehrere der folgenden untergeordneten Elemente enthalten:

-   Using
-   EntityContainer
-   EntityType
-   EnumType
-   Zuordnung
-   ComplexType
-   Funktion

Ein **Schema** Element darf keinem oder einem Anmerkungselemente enthalten.

> [!NOTE]
> Die **Funktion** -Element und Anmerkungselemente sind nur zulässig, in der CSDL-v2 und höher.

 

Die **Schema** -Element verwendet die **Namespace** Attribut, um den Namespace für den Entitätstyp, den komplexen Typ und die Zuordnungsobjekte in einem konzeptionellen Modell zu definieren. Innerhalb eines Namespace müssen alle Objekte eine eindeutige Bezeichnung aufweisen. Namespaces können mehrere umfassen **Schema** Elemente und mehrere CSDL-Dateien.

Namespace in einem konzeptionellen Modell unterscheidet sich von den XML-Namespace die **Schema** Element. Namespace in einem konzeptionellen Modell (gemäß der **Namespace** Attribut) ist ein logischer Container für Entitätstypen, komplexe Typen und Zuordnungstypen. Der XML-Namespace (angegeben durch die **Xmlns** Attribut) des eine **Schema** Element ist der Standardnamespace für untergeordnete Elemente und Attribute des der **Schema** Element. XML-Namespaces im Format http://schemas.microsoft.com/ado/YYYY/MM/edm (, wobei YYYY und MM ein Jahr und Monat darstellen) sind für CSDL reserviert. Benutzerdefinierte Elemente und Attribute können nicht in Namespaces mit diesem Format vorhanden sein.

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute angewendet werden können, um die **Schema** Element.

| Attributname | Ist erforderlich | Wert                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**  | Ja         | Der Namespace für das konzeptionelle Modell. Der Wert des der **Namespace** Attribut wird verwendet, um den vollqualifizierten Namen eines Typs zu bilden. Z. B. wenn ein **EntityType** mit dem Namen *Kunden* befindet sich im Simple.Example.Model-Namespace, und klicken Sie dann auf den vollqualifizierten Namen des der **EntityType** ist SimpleExampleModel.Customer. <br/> Die folgenden Zeichenfolgen können nicht verwendet werden, als Wert für die **Namespace** Attribut: **System**, **vorübergehende**, oder **Edm**. Der Wert für die **Namespace** Attribut kann nicht übereinstimmen, den der Wert für die **Namespace** Attribut im SSDL-Schema-Element. |
| **Alias**      | Nein          | Ein anstelle der Namespacebezeichnung verwendeter Bezeichner. Z. B. wenn ein **EntityType** mit dem Namen *Kunden* befindet sich im Simple.Example.Model-Namespace und den Wert des der **Alias** Attribut *Modell*, dann kann "Modell.Kunde" als den vollqualifizierten Namen der können Sie die **EntityType.**                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Schema** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **Schema** -Element mit einem **EntityContainer** Element, zwei **EntityType** -Elemente und ein **Zuordnung** Element.

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
 

 

## <a name="typeref-element-csdl"></a>TypeRef-Element (CSDL)

Die **TypeRef** -Element in konzeptioneller Schemadefinitionssprache (CSDL) stellt einen Verweis auf einen vorhandenen benannten Typ bereit. Die **TypeRef** Element kann sein, ein untergeordnetes Element des CollectionType-Element, das verwendet wird, um anzugeben, dass eine Funktion eine Auflistung als ein Parameter oder Rückgabetyp verfügt.

Ein **TypeRef** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element)
-   Anmerkungselemente (null oder mehr Elemente)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **TypeRef** Element. Beachten Sie, dass die **DefaultValue**, **MaxLength**, **FixedLength**, **Genauigkeit**, **Skalierung**,  **Unicode**, und **Sortierreihenfolge** Attribute gelten nur für **abzielt**.

| Attributname                                                     | Ist erforderlich | Wert                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**                                                           | Nein          | Der Name der Typbibliothek, auf die verwiesen wird.                                                                                                                                                                                          |
| **NULL-Werte zulässt**                                                       | Nein          | **"True"** (Standardwert) oder **"false"** abhängig davon, ob die Eigenschaft einen null-Wert haben kann. <br/> [!NOTE]                                                                                                                |
| > In CSDL v1 Eigenschaft eines komplexen Typs benötigen `Nullable="False"`. |             |                                                                                                                                                                                                                                 |
| **DefaultValue**                                                   | Nein          | Der Standardwert der Eigenschaft.                                                                                                                                                                                              |
| **MaxLength**                                                      | Nein          | Die maximale Länge des Eigenschaftswerts.                                                                                                                                                                                       |
| **FixedLength**                                                    | Nein          | **"True"** oder **"false"** abhängig davon, ob der Eigenschaftswert als Zeichenfolge mit fester Länge gespeichert wird.                                                                                                                          |
| **Genauigkeit**                                                      | Nein          | Die Genauigkeit des Eigenschaftswerts.                                                                                                                                                                                            |
| **Scale** (Skalieren)                                                          | Nein          | Die Skalierung des Eigenschaftswerts.                                                                                                                                                                                                |
| **SRID**                                                           | Nein          | Räumliche Verweis Systembezeichner. Gilt nur für Eigenschaften des räumlichen Typen. Weitere Informationen finden Sie unter [SRID](http://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **Unicode**                                                        | Nein          | **"True"** oder **"false"** abhängig davon, ob der Eigenschaftswert als Unicode-Zeichenfolge gespeichert wird.                                                                                                                               |
| **Sortierung**                                                      | Nein          | Eine Zeichenfolge, die die Sortierreihenfolge angibt, die in der Datenquelle verwendet werden soll.                                                                                                                                                   |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **CollectionType** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine modelldefinierte Funktion, verwendet der **TypeRef** Element (als untergeordnetes Element eine **CollectionType** Element) angeben, dass die Funktion eine Auflistung von akzeptiert  **Abteilung** Entitätstypen.

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
 

 

## <a name="using-element-csdl"></a>Using-Element (CSDL)

Die **Using** -Element in konzeptioneller Schemadefinitionssprache (CSDL) importiert den Inhalt eines konzeptionellen Modells, das in einem anderen Namespace vorhanden ist. Durch Festlegen des Werts, der die **Namespace** Attribut Sie verweisen auf Entitätstypen, komplexe Typen und Zuordnungstypen, die in einem anderen konzeptionellen Modell definiert sind. Mehr als eine **Using** Element ein untergeordnetes Element des kann sein, eine **Schema** Element.

> [!NOTE]
> Die **verwenden** -Element in CSDL funktioniert nicht genau so wie eine **mit** -Anweisung in einer Programmiersprache. Durch Importieren eines Namespace mit einem **mit** Anweisung in einer Programmiersprache, Sie wirken sich nicht die Objekte im ursprünglichen Namespace. In CSDL kann ein importierter Namespace einen Entitätstyp enthalten, der von einem Entitätstyp im ursprünglichen Namespace abgeleitet ist. Dies kann sich auf im ursprünglichen Namespace deklarierte Entitätssätze auswirken.

 

Die **Using** -Element kann die folgenden untergeordneten Elemente aufweisen:

-   Dokumentation (0 (null) oder ein Element zugelassen)
-   Anmerkungselemente (0 (null) oder mehrere Elemente zugelassen)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute angewendet werden können, um die **verwenden** Element.

| Attributname | Ist erforderlich | Wert                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**  | Ja         | Der Name des importierten Namespaces.                                                                                                                                                |
| **Alias**      | Ja         | Ein anstelle der Namespacebezeichnung verwendeter Bezeichner. Obwohl dieses Attribut erforderlich ist, muss es nicht anstelle des Namespacenamens verwendet wird, um Objektnamen zu qualifizieren. |

 

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Using** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

 

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt die **Using** Element verwendet wird, um einen Namespace zu importieren, die an anderer Stelle, definiert. Beachten Sie, dass der Namespace für die **Schema** Element ist das `BooksModel`. Die `Address` Eigenschaft für die `Publisher` **EntityType** ist ein komplexer Typ, der in definiert ist die `ExtendedBooksModel` Namespace (mit importiert die **verwenden** Element).

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
 

 

## <a name="annotation-attributes-csdl"></a>Anmerkungsattribute (CSDL)

Anmerkungsattribute in konzeptioneller Schemadefinitionssprache (CSDL) sind benutzerdefinierte XML-Attribute im konzeptionellen Modell. Neben dem Vorhandensein einer gültigen XML-Struktur muss für Anmerkungsattribute folgendes zutreffen:

-   Anmerkungsattribute dürfen sich in keinem XML-Namespace befinden, der für CSDL reserviert ist.
-   Für ein angegebenes CSDL-Element kann mehr als ein Anmerkungsattribut übernommen werden.
-   Die vollqualifizierten Namen zweier Anmerkungsattribute dürfen nicht übereinstimmen.

Anmerkungsattribute können verwendet werden, um zusätzliche Metadaten für die Elemente in einem konzeptionellen Modell bereitzustellen. Metadaten in Anmerkungselementen kann zur Laufzeit zugegriffen werden, mithilfe von Klassen im Namespace System.Data.Metadata.Edm.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **EntityType** Element mit einem anmerkungsattribut (**CustomAttribute**). Das Beispiel zeigt außerdem ein Anmerkungselement, das auf das Entitätstypelement angewendet wird.

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
 

Im folgenden Code werden die Metadaten im Anmerkungsattribut abgerufen und in die Konsole geschrieben:

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
 

Im Code oben wird davon ausgegangen, dass sich die Datei `School.csdl` im Ausgabeverzeichnis des Projekts befindet und dass Sie dem Projekt die folgenden `Imports`- und `Using`-Anweisungen hinzugefügt haben:

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a>Anmerkungselemente (CSDL)

Anmerkungselemente in konzeptioneller Schemadefinitionssprache (CSDL) sind benutzerdefinierte XML-Elemente im konzeptionellen Modell. Neben dem Vorhandensein einer gültigen XML-Struktur muss für Anmerkungselemente Folgendes zutreffen:

-   Anmerkungselemente dürfen sich in keinem XML-Namespace befinden, der für CSDL reserviert ist.
-   Mehrere Anmerkungselemente können untergeordnete Elemente eines angegebenen CSDL-Elements sein.
-   Die vollqualifizierten Namen zweier Anmerkungselemente dürfen nicht übereinstimmen.
-   Anmerkungselemente müssen nach allen anderen untergeordneten Elementen eines angegebenen CSDL-Elements angezeigt werden.

Anmerkungselemente können verwendet werden, um zusätzliche Metadaten für die Elemente in einem konzeptionellen Modell bereitzustellen. Ab .NET Framework, Version 4, kann in Anmerkungselementen enthaltene Metadaten zur Laufzeit zugegriffen werden mithilfe von Klassen im Namespace System.Data.Metadata.Edm.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **EntityType** Element mit einem Anmerkungselement (**CustomElement**). Das Beispiel zeigt außerdem ein Anmerkungsattribut, das auf das Entitätstypelement angewendet wird.

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
 

Im folgenden Code werden die Metadaten im Anmerkungselement abgerufen und in die Konsole geschrieben:

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
 

Im Code oben wird davon ausgegangen, dass sich die Datei School.csdl im Ausgabeverzeichnis des Projekts befindet, und dass Sie dem Projekt die folgenden `Imports`- und `Using`-Anweisungen hinzugefügt haben:

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a>Konzeptionelle Modelltypen (CSDL)

Konzeptionelle Schemadefinitionssprache (CSDL) unterstützt einen Satz von abstrakten primitiven Datentypen mit dem Namen **abzielt**, definieren Sie Eigenschaften, die in einem konzeptionellen Modell. **Abzielt** sind Proxys für primitive Datentypen, die in der Speicher- oder Hostingumgebung unterstützt werden.

In der nachfolgenden Tabelle werden die von CSDL unterstützten primitiven Datentypen aufgeführt. Die Tabelle enthält außerdem die Facets, die auf die einzelnen angewendet werden können **EDMSimpleType**.

| EDMSimpleType                    | Beschreibung                                                | Anwendbare Facets                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| **Edm.Binary**                   | Enthält Binärdaten.                                      | MaxLength, FixedLength, Nullable, Default                                |
| **Edm.Boolean**                  | Enthält den Wert **"true"** oder **"false"**.                  | Nullable, Default                                                        |
| **Edm.Byte**                     | Enthält einen 8-Bit-Ganzzahlwert ohne Vorzeichen.                  | Precision, Nullable, Default                                             |
| **Edm.DateTime**                 | Stellt ein Datum und eine Uhrzeit dar.                                | Precision, Nullable, Default                                             |
| **Edm.DateTimeOffset**           | Enthält ein Datum und eine Uhrzeit als Offset in Minuten von GMT. | Precision, Nullable, Default                                             |
| **Edm.Decimal**                  | Enthält einen numerischen Wert mit fester Genauigkeit und festen Dezimalstellen.   | Precision, Nullable, Default                                             |
| **"Edm.Double"**                   | Enthält eine Gleitkommazahl mit 15 Stellen Genauigkeit   | Precision, Nullable, Default                                             |
| **Edm.Float**                    | Enthält eine Gleitkommazahl mit einer Genauigkeit von 7 Stellen.   | Precision, Nullable, Default                                             |
| **Edm.Guid**                     | Enthält einen eindeutigen 16-Byte-Bezeichner.                      | Precision, Nullable, Default                                             |
| **Edm.Int16**                    | Enthält einen 16-Bit-Ganzzahlwert mit Vorzeichen.                    | Precision, Nullable, Default                                             |
| **Int32**                    | Enthält einen 32-Bit-Ganzzahlwert mit Vorzeichen.                    | Precision, Nullable, Default                                             |
| **Int64**                    | Enthält einen 64-Bit-Ganzzahlwert mit Vorzeichen.                    | Precision, Nullable, Default                                             |
| **Edm.SByte**                    | Enthält einen 8-Bit-Ganzzahlwert mit Vorzeichen.                     | Precision, Nullable, Default                                             |
| **"Edm.String"**                   | Enthält Zeichendaten.                                   | Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default |
| **Edm.Time**                     | Enthält eine Uhrzeit.                                    | Precision, Nullable, Default                                             |
| **Edm.Geography**                |                                                            | NULL-Werte zulässt, Standard, SRID                                                  |
| **"Edm.geographypoint"**           |                                                            | NULL-Werte zulässt, Standard, SRID                                                  |
| **Edm.GeographyLineString**      |                                                            | NULL-Werte zulässt, Standard, SRID                                                  |
| **Edm.GeographyPolygon**         |                                                            | NULL-Werte zulässt, Standard, SRID                                                  |
| **Edm.GeographyMultiPoint**      |                                                            | NULL-Werte zulässt, Standard, SRID                                                  |
| **Edm.GeographyMultiLineString** |                                                            | NULL-Werte zulässt, Standard, SRID                                                  |
| **Edm.GeographyMultiPolygon**    |                                                            | NULL-Werte zulässt, Standard, SRID                                                  |
| **Edm.GeographyCollection**      |                                                            | NULL-Werte zulässt, Standard, SRID                                                  |
| **Edm.Geometry**                 |                                                            | NULL-Werte zulässt, Standard, SRID                                                  |
| **Edm.GeometryPoint**            |                                                            | NULL-Werte zulässt, Standard, SRID                                                  |
| **Edm.GeometryLineString**       |                                                            | NULL-Werte zulässt, Standard, SRID                                                  |
| **Edm.GeometryPolygon**          |                                                            | NULL-Werte zulässt, Standard, SRID                                                  |
| **Edm.GeometryMultiPoint**       |                                                            | NULL-Werte zulässt, Standard, SRID                                                  |
| **Edm.GeometryMultiLineString**  |                                                            | NULL-Werte zulässt, Standard, SRID                                                  |
| **Edm.GeometryMultiPolygon**     |                                                            | NULL-Werte zulässt, Standard, SRID                                                  |
| **Edm.GeometryCollection**       |                                                            | NULL-Werte zulässt, Standard, SRID                                                  |

## <a name="facets-csdl"></a>Facets (CSDL)

Facets in konzeptioneller Schemadefinitionssprache (CSDL) stellen Einschränkungen für Eigenschaften von Entitätstypen und komplexen Typen dar. Facets werden in den folgenden CSDL-Elementen als XML-Attribute angezeigt:

-   Eigenschaft
-   TypeRef
-   Parameter

In der folgenden Tabelle werden die in CSDL unterstützten Facets beschrieben. Alle Facets sind optional. Beim Generieren einer Datenbank aus einem konzeptionellen Modell, werden einige der unten aufgeführten Facets vom Entity Framework verwendet.

> [!NOTE]
> Informationen zu Datentypen in einem konzeptionellen Modell finden Sie konzeptionelle Modelltypen (CSDL).

| Facet               | Beschreibung                                                                                                                                                                                                                                                   | Betrifft                                                                                                                                                                                                                                                                                                                                                                           | Wird für die Datenbankgenerierung verwendet | Wird von der Laufzeit verwendet |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| **Sortierung**       | Gibt die bei Vergleich- und Sortiervorgängen zu verwendende Sortierreihenfolge für die Werte der Eigenschaft an.                                                                                                               | **"Edm.String"**                                                                                                                                                                                                                                                                                                                                                                       | Ja                              | Nein                  |
| **ConcurrencyMode** | Gibt an, dass der Eigenschaftswert für Prüfungen der vollständigen Parallelität verwendet werden soll.                                                                                                                                                                    | Alle **EDMSimpleType** Eigenschaften                                                                                                                                                                                                                                                                                                                                                     | Nein                               | Ja                 |
| **Default**         | Gibt den Standardwert der Eigenschaft an, wenn bei der Instanziierung kein Wert angegeben wird.                                                                                                                                                                       | Alle **EDMSimpleType** Eigenschaften                                                                                                                                                                                                                                                                                                                                                     | Ja                              | Ja                 |
| **FixedLength**     | Gibt an, ob sich die Länge des Eigenschaftswerts ändern kann.                                                                                                                                                                                                  | **Edm.Binary**, **"Edm.String"**                                                                                                                                                                                                                                                                                                                                                       | Ja                              | Nein                  |
| **MaxLength**       | Gibt die maximale Länge des Eigenschaftswerts an.                                                                                                                                                                                                           | **Edm.Binary**, **"Edm.String"**                                                                                                                                                                                                                                                                                                                                                       | Ja                              | Nein                  |
| **NULL-Werte zulässt**        | Gibt an, ob die Eigenschaft haben kann eine **null** Wert.                                                                                                                                                                                                     | Alle **EDMSimpleType** Eigenschaften                                                                                                                                                                                                                                                                                                                                                     | Ja                              | Ja                 |
| **Genauigkeit**       | Bei Eigenschaften vom Typ **Decimal**, gibt die Anzahl der Ziffern ein Eigenschaftswert verfügen kann. Bei Eigenschaften vom Typ **Zeit**, **"DateTime"**, und **DateTimeOffset**, gibt die Anzahl von Ziffern für die Sekundenbruchteile des Eigenschaftswerts. | **Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**                                                                                                                                                                                                                                                                                                              | Ja                              | Nein                  |
| **Scale** (Skalieren)           | Gibt die Anzahl der Dezimalstellen für den Eigenschaftswert an.                                                                                                                                                                      | **Edm.Decimal**                                                                                                                                                                                                                                                                                                                                                                      | Ja                              | Nein                  |
| **SRID**            | Gibt an, die räumliche Verweis System-ID Weitere Informationen finden Sie unter [SRID](http://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).                                                              | **Edm.Geography, "Edm.geographypoint", Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection** | Nein                               | Ja                 |
| **Unicode**         | Gibt an, ob der Eigenschaftswert als Unicode gespeichert wird.                                                                                                                                                                                                    | **"Edm.String"**                                                                                                                                                                                                                                                                                                                                                                       | Ja                              | Ja                 |

>[!NOTE]
> Beim Generieren einer Datenbank aus einem konzeptionellen Modell erkennt der Assistent zur Datenbankgenerierung den Wert des der **StoreGeneratedPattern** -Attribut für eine **Eigenschaft** Elements, sofern es in der folgenden Namespace: http://schemas.microsoft.com/ado/2009/02/edm/annotation. Die unterstützten Werte für das Attribut sind **Identität** und **berechnet**. Der Wert **Identität** erzeugt eine Datenbankspalte mit einem Identitätswert, der in der Datenbank generiert wird. Der Wert **berechnet** erzeugt eine Spalte mit einem Wert, der in der Datenbank berechnet wird.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt für die Eigenschaften eines Entitätstyps übernommene Facets:

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
