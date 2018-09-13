---
title: SSDL-Spezifikation - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: a4af4b1a-40f4-48cc-b2e0-fa8f5d9d5419
ms.openlocfilehash: a8b1f844a34c44d283982a52cef3bf80afd7e679
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490349"
---
# <a name="ssdl-specification"></a>SSDL-Spezifikation
Die Datenspeicherschema-Definitionssprache (Store Schema Definition Language, SSDL) ist eine XML-basierte Sprache, die das Speichermodell einer Entity Framework-Anwendung beschreibt.

In einer Entity Framework-Anwendung Metadaten des Speichermodells aus einer SSDL-Datei (geschrieben in SSDL) in eine Instanz von der System.Data.Metadata.Edm.StoreItemCollection geladen wird und kann mit Verwendung von Methoden in der System.Data.Metadata.Edm.MetadataWorkspace-Klasse. Entitätsframework verwendet Metadaten des Speichermodells, um Abfragen für das konzeptionelle Modell in datenspeicherspezifische Befehle zu übersetzen.

Die Entity Framework Designer (EF-Designer) speichert Informationen zum Speichermodell in einer EDMX-Datei zur Entwurfszeit. Zum Zeitpunkt der Erstellung verwendet der Entity Designer Informationen in einer EDMX-Datei aus, um die SSDL-Datei zu erstellen, die zur Laufzeit vom Entity Framework benötigt wird.

Die verschiedenen Versionen von SSDL werden durch XML-Namespaces unterschieden.

| SSDL-Version | XML-Namespace                                     |
|:-------------|:--------------------------------------------------|
| SSDL v1      | http://schemas.microsoft.com/ado/2006/04/edm/ssdl |
| SSDL-v2      | http://schemas.microsoft.com/ado/2009/02/edm/ssdl |
| SSDL v3      | http://schemas.microsoft.com/ado/2009/11/edm/ssdl |

## <a name="association-element-ssdl"></a>Zuordnungselement (SSDL)

Ein **Zuordnung** Element in der Datenspeicherschema-Definitionssprache (SSDL) gibt Tabellenspalten, die beteiligt sind, in eine foreign Key-Einschränkung in der zugrunde liegenden Datenbank. Geben Sie zwei erforderliche untergeordnete Endelemente Tabellen an den Enden der Zuordnung und die Multiplizität an jedem Ende. Ein optionales ReferentialConstraint-Element gibt das prinzipalende und das abhängige Ende der Zuordnung sowie die teilnehmenden Spalten an. Wenn kein **ReferentialConstraint** -Element vorhanden ist, AssociationSetMapping-Elements muss verwendet werden, um die spaltenzuordnungen für die Zuordnung anzugeben.

Die **Zuordnung** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element)
-   Ende (genau zwei Elemente)
-   ReferentialConstraint (0 (null) oder 1)
-   Anmerkungselemente (null oder mehr)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Zuordnung** Element.

| Attributname | Ist erforderlich | Wert                                                                            |
|:---------------|:------------|:---------------------------------------------------------------------------------|
| **Name**       | Ja         | Der Name der entsprechenden Fremdschlüsseleinschränkung in der zugrunde liegenden Datenbank. |

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Zuordnung** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **Zuordnung** -Element, das verwendet eine **ReferentialConstraint** Element, um die Spalten anzugeben, die Teilnahme an der **FK\_CustomerOrders**  foreign Key-Einschränkung:

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

## <a name="associationset-element-ssdl"></a>AssociationSet-Element (SSDL)

Die **AssociationSet** -Element in der Datenspeicherschema-Definitionssprache (SSDL) stellt eine fremdschlüsseleinschränkung zwischen zwei Tabellen in der zugrunde liegenden Datenbank dar. Die Tabellenspalten, die in der foreign Key-Einschränkung einbezogen werden in ein Association-Elements angegeben. Die **Zuordnung** Element, das entspricht, einer angegebenen **AssociationSet** Element wird angegeben, der **Zuordnung** Attribut der **AssociationSet**  Element.

SSDL-Zuordnungssätze werden CSDL-Zuordnungssätzen von einem AssociationSetMapping-Element zugeordnet. Wenn die CSDL-Zuordnung für eine bestimmte CSDL-Zuordnung festgelegt ist jedoch mit einem ReferentialConstraint-Element, das kein entsprechendes definiert **AssociationSetMapping** Element ist erforderlich. In diesem Fall wenn ein **AssociationSetMapping** -Element vorhanden ist, werden von der die Zuordnungen definiert überschrieben der **ReferentialConstraint** Element.

Die **AssociationSet** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element)
-   Ende (0 (null) oder zwei)
-   Anmerkungselemente (null oder mehr)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **AssociationSet** Element.

| Attributname  | Ist erforderlich | Wert                                                                                                |
|:----------------|:------------|:-----------------------------------------------------------------------------------------------------|
| **Name**        | Ja         | Der Name der Fremdschlüsseleinschränkung, die der Zuordnungssatz darstellt.                          |
| **Zuordnung** | Ja         | Der Name der Zuordnung, die die Spalten definiert, die an der Fremdschlüsseleinschränkung teilnehmen. |

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **AssociationSet** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **AssociationSet** Element und die repräsentiert die `FK_CustomerOrders` foreign Key-Einschränkung in der zugrunde liegenden Datenbank:

``` xml
 <AssociationSet Name="FK_CustomerOrders"
                 Association="ExampleModel.Store.FK_CustomerOrders">
   <End Role="Customers" EntitySet="Customers" />
   <End Role="Orders" EntitySet="Orders" />
 </AssociationSet>
```

## <a name="collectiontype-element-ssdl"></a>CollectionType-Element (SSDL)

Die **CollectionType** Element in der Datenspeicherschema-Definitionssprache (SSDL) gibt an, dass ein Funktionsrückgabetyp eine Auflistung. Die **CollectionType** Element ist ein untergeordnetes Element des ReturnType-Element. Der Typ der Auflistung wird angegeben, mit dem untergeordneten RowType-Element:

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **CollectionType** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine Funktion, verwendet eine **CollectionType** Element, um anzugeben, dass die Funktion eine Auflistung von Zeilen zurückgibt.

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

## <a name="commandtext-element-ssdl"></a>CommandText-Element (SSDL)

Die **CommandText** Element in der Datenspeicherschema-Definitionssprache (SSDL) ist ein untergeordnetes Element des Function-Element, mit dem Sie eine SQL-Anweisung zu definieren, die in der Datenbank ausgeführt wird. Die **CommandText** -Element können Sie zum Hinzufügen von Funktionen, die an eine gespeicherte Prozedur in der Datenbank ähnelt, aber Sie definieren die **CommandText** -Element im Speichermodell.

Die **CommandText** Element darf keine untergeordneten Elemente. Der Hauptteil der **CommandText** Element muss eine gültige SQL­Anweisung für die zugrunde liegende Datenbank sein.

Es sind keine Attribute für die **CommandText** Element.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **Funktion** -Element mit einem untergeordneten **CommandText** Element. Bereitstellen der **UpdateProductInOrder** Funktion als Methode für den ObjectContext durch in das konzeptionelle Modell importieren.  

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

## <a name="definingquery-element-ssdl"></a>DefiningQuery-Element (SSDL)

Die **DefiningQuery** Element in der Datenspeicherschema-Definitionssprache (SSDL) können Sie eine SQL-Anweisung direkt in der zugrunde liegenden Datenbank ausführen. Die **DefiningQuery** Element wird häufig verwendet, wie eine Datenbanksicht, aber die Ansicht im Speichermodell nicht in der Datenbank definiert ist. Die Sicht definiert, die einem **DefiningQuery** Element eines Entitätstyps im konzeptionellen Modell über ein EntitySetMapping-Element zugeordnet werden kann. Diese Zuordnungen sind schreibgeschützt.  

Die folgende SSDL-Syntax zeigt die Deklaration einer **EntitySet** gefolgt von der **DefiningQuery** Element, das eine Abfrage zum Abrufen der Sicht enthält.

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

Sie können gespeicherte Prozeduren zum Aktivieren der Lese-/ schreibszenarien über Ansichten im Entity Framework verwenden. Sie können entweder eine Datenquellensicht oder eine Entity SQL-Sicht als Basistabelle zum Abrufen von Daten und zum Verarbeiten von Änderungen durch gespeicherte Prozeduren verwenden.

Sie können die **DefiningQuery** Element in Microsoft SQL Server Compact 3.5 als Ziel. Obwohl SQL Server Compact 3.5 keine gespeicherte Prozeduren unterstützt, können Sie ähnliche Funktionalität mit implementieren die **DefiningQuery** Element. Es kann auch beim Erstellen gespeicherter Prozeduren nützlich sein, um einen Konflikt zwischen den in der Programmiersprache und den in der Datenquelle verwendeten Datentypen zu lösen. Sie könnten eine **DefiningQuery** , die einen bestimmten Satz von Parametern akzeptiert und ruft dann eine gespeicherte Prozedur mit einem anderen Satz von Parametern, z. B. eine gespeicherte Prozedur, die Daten löscht.

## <a name="dependent-element-ssdl"></a>Abhängiges Element (SSDL)

Die **abhängige** Element in der Datenspeicherschema-Definitionssprache (SSDL) ist ein untergeordnetes Element mit dem ReferentialConstraint-Element, das abhängige Ende einer fremdschlüsseleinschränkung (auch referenzielle Einschränkung genannt) definiert. Die **abhängige** Element gibt die Spalte (oder Spalten) in einer Tabelle, die eine primäre Schlüsselspalte (oder Spalten) zu verweisen. **PropertyRef** Elemente anzugeben, welche Spalten verwiesen werden. Der Prinzipal Element gibt die Primärschlüsselspalten, die durch Spalten, die im angegebenen verwiesen werden die **abhängige** Element.

Die **abhängige** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   PropertyRef (mindestens)
-   Anmerkungselemente (null oder mehr)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **abhängige** Element.

| Attributname | Ist erforderlich | Wert                                                                                                                                                       |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Rolle**       | Ja         | Der gleiche Wert wie die **Rolle** Attributs (sofern verwendet) des entsprechenden Elements Ende; andernfalls der Name der Tabelle enthält, die die verweisende Spalte. |

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **abhängige** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt ein Association-Element, das verwendet eine **ReferentialConstraint** Element, um die Spalten anzugeben, die Teilnahme an der **FK\_CustomerOrders** Fremdschlüssel Einschränkung. Die **abhängige** -Element gibt an die **"CustomerID"** Spalte die **Reihenfolge** Tabelle als das abhängige Ende der Einschränkung.

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

## <a name="documentation-element-ssdl"></a>Documentation-Element (SSDL)

Die **Dokumentation** -Element in der Datenspeicherschema-Definitionssprache (SSDL) verwendet werden kann, um Informationen zu einem Objekt bereitzustellen, die in einem übergeordneten Element definiert ist.

Die **Dokumentation** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   **Zusammenfassung**: eine kurze Beschreibung des übergeordneten Elements. (kein (Null) oder ein Element)
-   **LongDescription**: eine ausführliche Beschreibung des übergeordneten Elements. (kein (Null) oder ein Element)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Dokumentation** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt die **Dokumentation** Element als untergeordnetes Element eines EntityType-Elements.

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

## <a name="end-element-ssdl"></a>End-Element (SSDL)

Die **End** Element in der Datenspeicherschema-Definitionssprache (SSDL) gibt die Tabelle und die Anzahl der Zeilen an einem Ende einer fremdschlüsseleinschränkung in der zugrunde liegenden Datenbank an. Die **End** Element kann ein untergeordnetes Element des Association-Element oder das AssociationSet-Element sein. In jedem dieser Fälle sind andere untergeordnete Elemente und anwendbare Attribute die möglich.

### <a name="end-element-as-a-child-of-the-association-element"></a>Das End-Element als untergeordnetes Objekt des Association-Elements

Ein **End** Element (als untergeordnetes Element der **Zuordnung** Element) gibt die Tabelle und die Anzahl der Zeilen am Ende einer foreign Key-Einschränkung mit der **Typ** und **Multiplizität** -Attribut an. Die Enden einer Fremdschlüsseleinschränkung sind als Teil einer SSDL-Zuordnung definiert. Eine SSDL-Zuordnung muss genau zwei Enden aufweisen.

Ein **End** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element)
-   OnDelete (0 (null) oder ein Element)
-   Anmerkungselemente (null oder mehr Elemente)

#### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **End** -Element, wenn es sich um das untergeordnete Element ist ein **Zuordnung** Element.

| Attributname   | Ist erforderlich | Wert                                                                                                                                                                                                                                                                                                                                                                                      |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Type**         | Ja         | Der voll gekennzeichnete Name der SSDL-Entitätenmenge, die am Ende der foreign Key-Einschränkung ist.                                                                                                                                                                                                                                                                                          |
| **Rolle**         | Nein          | Der Wert des der **Rolle** -Attribut im dem Prinzipal- oder abhängigen Element von der entsprechenden ReferentialConstraint-Element (sofern verwendet).                                                                                                                                                                                                                                             |
| **Multiplizität** | Ja         | **1**, **0.. 1**, oder **\*** abhängig von der Anzahl der Zeilen, die am Ende der foreign Key-Einschränkung sein können. <br/> **1** gibt an, dass genau eine Zeile, die am Ende foreign Key-Einschränkung vorhanden ist. <br/> **0.. 1** gibt an, dass maximal eine Zeile, die am Ende foreign Key-Einschränkung vorhanden ist. <br/> **\*** Gibt an, dass 0 (null), einer oder mehreren Zeilen am Ende foreign Key-Einschränkung vorhanden sind. |

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **End** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

#### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **Zuordnung** -Element, definiert der **FK\_CustomerOrders** foreign Key-Einschränkung. Die **Multiplizität** auf jedem angegebenen Werte **End** Element anzugeben, dass viele Zeilen in der **Bestellungen** Tabelle kann mit einer Zeile im verbunden werden die **Kunden**  Tabelle, jedoch nur eine Zeile in der **Kunden** Tabelle möglich mit einer Zeile in der **Bestellungen** Tabelle. Darüber hinaus die **OnDelete** Element gibt an, dass alle Zeilen in der **Bestellungen** Tabelle, die eine bestimmte Zeile in verweisen auf die **Kunden** Tabelle gelöscht werden, wenn die Zeile in die **Kunden** Tabelle gelöscht wird.

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

### <a name="end-element-as-a-child-of-the-associationset-element"></a>Das End-Element als untergeordnetes Objekt des AssociationSet-Elements

Die **End** -Element (als untergeordnetes Element der **AssociationSet** Element) gibt eine Tabelle an einem Ende einer fremdschlüsseleinschränkung in der zugrunde liegenden Datenbank.

Ein **End** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element)
-   Anmerkungselemente (null oder mehr)

#### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **End** -Element, wenn es sich um das untergeordnete Element ist ein **AssociationSet** Element.

| Attributname | Ist erforderlich | Wert                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------|
| **EntitySet**  | Ja         | Der Name der SSDL-Entitätenmenge, die am Ende der foreign Key-Einschränkung ist.                                      |
| **Rolle**       | Nein          | Der Wert eines der **Rolle** auf einem angegebenen Attribute **End** Element des entsprechenden Association-Elements. |

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **End** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

#### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **EntityContainer** -Element mit einem **AssociationSet** -Element mit zwei **End** Elemente:

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

## <a name="entitycontainer-element-ssdl"></a>EntityContainer-Element (SSDL)

Ein **EntityContainer** Element in der Datenspeicherschema-Definitionssprache (SSDL) beschreibt die Struktur der zugrunde liegenden Datenquelle in einer Entity Framework-Anwendung: SSDL-Entitätenmengen (definiert in EntitySet-Elemente) stellen Tabellen dar. eine Datenbank, SSDL-Entitätstypen (definiert im EntityType-Elemente) Zeilen in einer Tabelle und Zuordnungssätze (definiert in AssociationSet-Elementen) darstellen foreign Key-Einschränkungen in einer Datenbank. Ein Speichermodell-Entitätscontainer wird über die EntityContainerMapping-Element ein konzeptioneller modellentitätscontainer zugeordnet.

Ein **EntityContainer** -Element kann NULL oder eine dokumentationselemente aufweisen. Wenn eine **Dokumentation** -Element vorhanden ist, sie müssen vor allen anderen untergeordneten Elementen stehen.

Ein **EntityContainer** -Element kann 0 (null) oder mehrere der folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet) aufweisen:

-   EntitySet
-   AssociationSet
-   Anmerkungselemente

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **EntityContainer** Element.

| Attributname | Ist erforderlich | Wert                                                                   |
|:---------------|:------------|:------------------------------------------------------------------------|
| **Name**       | Ja         | Der Name des Entitätscontainers. Dieser Name darf keine Punkte (.) enthalten. |

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **EntityContainer** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **EntityContainer** Element, das zwei Entitätenmengen und einen Zuordnungssatz definiert. Beachten Sie, dass die Namen des Entitätstyps und des Zuordnungstyps mit dem Namespace des konzeptionellen Modellnamens qualifiziert werden.

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

## <a name="entityset-element-ssdl"></a>EntitySet-Element (SSDL)

Ein **EntitySet** -Element der Datenspeicherschema-Definitionssprache (SSDL) stellt dar, eine Tabelle oder Sicht in der zugrunde liegenden Datenbank. Ein EntityType-Element in SSDL stellt eine Zeile in der Tabelle oder Sicht dar. Die **EntityType** Attribut eine **EntitySet** Element gibt an, die bestimmten SSDL-Entitätstyp, der Zeilen in einer SSDL-Entitätenmenge darstellt. Die Zuordnung zwischen einer CSDL-Entitätenmenge und einer SSDL-Entitätenmenge, die in einem EntitySetMapping-Element angegeben ist.

Die **EntitySet** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element)
-   DefiningQuery (0 (null) oder ein Element)
-   Anmerkungselemente

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **EntitySet** Element.

> [!NOTE]
> Einige Attribute (hier nicht aufgeführt) qualifiziert werden können, mit der **speichern** Alias. Diese Attribute werden durch den Modellaktualisierungs-Assistenten verwendet, wenn ein Modell aktualisieren.

| Attributname | Ist erforderlich | Wert                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| **Name**       | Ja         | Der Name des Entitätssatzes.                                                              |
| **EntityType** | Ja         | Der vollqualifizierte Name des Entitätstyps, für den der Entitätssatz Instanzen enthält. |
| **Schema**     | Nein          | Das Datenbankschema.                                                                     |
| **Table**      | Nein          | Die Datenbanktabelle.                                                                      |

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **EntitySet** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **EntityContainer** -Element, das zwei **EntitySet** Elemente und ein **AssociationSet** Element:

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

## <a name="entitytype-element-ssdl"></a>EntityType-Element (SSDL)

Ein **EntityType** Element in der Datenspeicherschema-Definitionssprache (SSDL) stellt eine Zeile in einer Tabelle oder Sicht der zugrunde liegenden Datenbank dar. Ein EntitySet-Element in SSDL stellt dar, die Tabelle oder Sicht, die in der Zeilen enthalten. Die **EntityType** Attribut eine **EntitySet** Element gibt an, die bestimmten SSDL-Entitätstyp, der Zeilen in einer SSDL-Entitätenmenge darstellt. Die Zuordnung zwischen einer SSDL-Entitätstyp und einer CSDL-Entitätstyp wird in einem EntityTypeMapping-Element angegeben.

Die **EntityType** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element)
-   Schlüssel (0 (null) oder ein Element)
-   Anmerkungselemente

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **EntityType** Element.

| Attributname | Ist erforderlich | Wert                                                                                                                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**       | Ja         | Der Name des Entitätstyps. Dieser Wert ist normalerweise der gleiche wie der Name der Tabelle, in der der Entitätstyp eine Zeile darstellt. Dieser Wert darf keine Punkte (.) enthalten. |

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **EntityType** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **EntityType** -Element mit zwei Eigenschaften:

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

## <a name="function-element-ssdl"></a>Function-Element (SSDL)

Die **Funktion** Element in der Datenspeicherschema-Definitionssprache (SSDL) gibt eine vorhandene gespeicherte Prozedur in der zugrunde liegenden Datenbank.

Die **Funktion** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element)
-   Parameter (null oder mehr)
-   CommandText-Eigenschaft (0 (null) oder 1)
-   ReturnType (null oder mehr)
-   Anmerkungselemente (null oder mehr)

Ein Rückgabewert für eine Funktion muss angegeben werden, entweder mit der **ReturnType** Element oder die **ReturnType** -Attribut (siehe unten), aber nicht beides.

Gespeicherte Prozeduren, die im Speichermodell angegeben sind, können in das konzeptionelle Modell einer Anwendung importiert werden. Weitere Informationen finden Sie unter [Abfragen mit gespeicherten Prozeduren](~/ef6/modeling/designer/stored-procedures/query.md). Die **Funktion** -Element kann auch verwendet werden, um benutzerdefinierte Funktionen im Speichermodell zu definieren.  

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Funktion** Element.

> [!NOTE]
> Einige Attribute (hier nicht aufgeführt) qualifiziert werden können, mit der **speichern** Alias. Diese Attribute werden durch den Modellaktualisierungs-Assistenten verwendet, wenn ein Modell aktualisieren.

| Attributname             | Ist erforderlich | Wert                                                                                                                                                                                                              |
|:---------------------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                   | Ja         | Der Name der gespeicherten Prozedur.                                                                                                                                                                                  |
| **ReturnType**             | Nein          | Der Rückgabetyp der gespeicherten Prozedur.                                                                                                                                                                           |
| **Aggregat**              | Nein          | **"True"** , wenn die gespeicherte Prozedur einen Aggregatwert zurückgibt. andernfalls **"false"**.                                                                                                                                  |
| **"Vordefiniert"**                | Nein          | **"True"** ist die Funktion eine integrierte<sup>1</sup> funktioniert; andernfalls **"false"**.                                                                                                                                  |
| **StoreFunctionName**      | Nein          | Der Name der gespeicherten Prozedur.                                                                                                                                                                                  |
| **NiladicFunction**        | Nein          | **"True"** ist die Funktion eine Niladic<sup>2</sup> Funktion. **"False"** andernfalls.                                                                                                                                   |
| **IsComposable**           | Nein          | **"True"** ist die Funktion eine zusammensetzbare<sup>3</sup> Funktion. **"False"** andernfalls.                                                                                                                                |
| **ParameterTypeSemantics** | Nein          | Die Enumeration, die die Typsemantik definiert, die zum Auflösen von Funktionsüberladungen verwendet wird. Die Enumeration ist im Anbietermanifest für jede Funktionsdefinition definiert. Der Standardwert ist **AllowImplicitConversion**. |
| **Schema**                 | Nein          | Der Name des Schemas, in dem die gespeicherte Prozedur definiert ist.                                                                                                                                                   |

<sup>1</sup> eine integrierte Funktion ist eine Funktion, die in der Datenbank definiert ist. Informationen zu Funktionen, die im Speichermodell definiert sind, finden Sie unter CommandText-Element (SSDL).

<sup>2</sup> eine Niladic-Funktion ist eine Funktion, die keine Parameter akzeptiert und bei Aufruf keine Klammern erforderlich.

<sup>3</sup> zwei Funktionen sind zusammensetzbar, wenn die Ausgabe einer Funktion die Eingabe für die andere Funktion werden kann.

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Funktion** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **Funktion** Element, das entspricht, dem **UpdateOrderQuantity** gespeicherte Prozedur. Die gespeicherte Prozedur akzeptiert zwei Parameter und gibt keinen Wert zurück.

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

## <a name="key-element-ssdl"></a>Key-Element (SSDL)

Die **Schlüssel** -Element der Datenspeicherschema-Definitionssprache (SSDL) stellt den Primärschlüssel einer Tabelle in der zugrunde liegenden Datenbank dar. **Schlüssel** ist ein untergeordnetes Element des EntityType-Element, das eine Zeile in einer Tabelle darstellt. Der Primärschlüssel wird definiert, der **Schlüssel** Element durch einen Verweis auf ein oder mehrere Eigenschaft-Elemente, die auf definiert sind die **EntityType** Element.

Die **Schlüssel** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   PropertyRef (mindestens)
-   Anmerkungselemente

Es sind keine Attribute für die **Schlüssel** Element.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **EntityType** Elements mit einem Schlüssel, der eine Eigenschaft verweist:

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

## <a name="ondelete-element-ssdl"></a>OnDelete-Element (SSDL)

Die **OnDelete** Element in der Datenspeicherschema-Definitionssprache (SSDL) spiegelt das Datenbankverhalten wider, wenn eine Zeile, die Teil einer foreign Key-Einschränkung gelöscht wird. Wenn die Aktion, um festgelegt ist **Cascade**, und klicken Sie dann die Zeilen, die eine Zeile verweisen, die gelöscht wird ebenfalls gelöscht werden. Wenn die Aktion, um festgelegt ist **keine**, und klicken Sie dann die Zeilen, die eine Zeile verweisen, die gelöscht wird, nicht gelöscht. Ein **OnDelete** Element ist ein untergeordnetes Element eines End-Elements.

Ein **OnDelete** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element)
-   Anmerkungselemente (null oder mehr)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **OnDelete** Element.

| Attributname | Ist erforderlich | Wert                                                                                               |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------|
| **Aktion**     | Ja         | **CASCADE** oder **keine**. (Der Wert **Restricted** ist gültig, aber hat das gleiche Verhalten wie **keine**.) |

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **OnDelete** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **Zuordnung** -Element, definiert der **FK\_CustomerOrders** foreign Key-Einschränkung. Die **OnDelete** Element gibt an, dass alle Zeilen in der **Bestellungen** Tabelle, die eine bestimmte Zeile in verweisen auf die **Kunden** Tabelle gelöscht werden, wenn die Zeile in der **Kunden** Tabelle gelöscht wird.

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

## <a name="parameter-element-ssdl"></a>Parameter-Element (SSDL)

Die **Parameter** Element in der Datenspeicherschema-Definitionssprache (SSDL) ist ein untergeordnetes Element des Function-Element, das Parameter für eine gespeicherte Prozedur in der Datenbank angibt.

Die **Parameter** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   Dokumentation (0 (null) oder ein Element)
-   Anmerkungselemente (null oder mehr)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Parameter** Element.

| Attributname | Ist erforderlich | Wert                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**       | Ja         | Der Name des Parameters.                                                                                                                                                                                                      |
| **Type**       | Ja         | Der Typ des Parameters.                                                                                                                                                                                                             |
| **Modus**       | Nein          | **In**, **Out**, oder **InOut** abhängig davon, ob der Parameter eine Eingabe, Ausgabe oder Eingabe-/Ausgabeparameter.                                                                                                                |
| **MaxLength**  | Nein          | Die maximale Länge des Parameters.                                                                                                                                                                                            |
| **Genauigkeit**  | Nein          | Die Genauigkeit des Parameters.                                                                                                                                                                                                 |
| **Scale** (Skalieren)      | Nein          | Die Skalierung des Parameters.                                                                                                                                                                                                     |
| **SRID**       | Nein          | Räumliche Verweis Systembezeichner. Gilt nur für Parameter des räumlichen Typen. Weitere Informationen finden Sie unter [SRID](http://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Parameter** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **Funktion** -Element, das zwei **Parameter** Elemente, die Eingabeparameter angeben:

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

## <a name="principal-element-ssdl"></a>Prinzipalelement (SSDL)

Die **Principal** Element in der Datenspeicherschema-Definitionssprache (SSDL) ist ein untergeordnetes Element mit dem ReferentialConstraint-Element, das prinzipalende einer fremdschlüsseleinschränkung (auch referenzielle Einschränkung genannt) definiert. Die **Principal** Element gibt die Primärschlüsselspalte (oder Spalten) in einer Tabelle, die von einer anderen Spalte (oder Spalten) verwiesen wird. **PropertyRef** Elemente anzugeben, welche Spalten verwiesen werden. Dependent-Element gibt die Spalten, die die Primärschlüsselspalten, die im angegebenen verweisen die **Principal** Element.

Die **Principal** Element haben die folgenden untergeordneten Elemente (in entsprechender Reihenfolge aufgelistet):

-   PropertyRef (mindestens)
-   Anmerkungselemente (null oder mehr)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Principal** Element.

| Attributname | Ist erforderlich | Wert                                                                                                                                                      |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Rolle**       | Ja         | Der gleiche Wert wie die **Rolle** Attributs (sofern verwendet) des entsprechenden Elements Ende; andernfalls der Name der Tabelle enthält die Spalte, auf die verwiesen wird. |

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Principal** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt ein Association-Element, das verwendet eine **ReferentialConstraint** Element, um die Spalten anzugeben, die Teilnahme an der **FK\_CustomerOrders** Fremdschlüssel Einschränkung. Die **Principal** -Element gibt an die **"CustomerID"** Spalte die **Kunden** Tabelle als das prinzipalende der Einschränkung.

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

## <a name="property-element-ssdl"></a>Property-Element (SSDL)

Die **Eigenschaft** Element in der Datenspeicherschema-Definitionssprache (SSDL) stellt eine Spalte in einer Tabelle in der zugrunde liegenden Datenbank dar. **Eigenschaft** Elemente sind untergeordnete Elemente des EntityType-Elemente, die Zeilen in einer Tabelle darstellen. Jede **Eigenschaft** Element definiert eine **EntityType** -Element stellt eine Spalte dar.

Ein **Eigenschaft** Element kann keine untergeordneten Elemente aufweisen.

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **Eigenschaft** Element.

| Attributname            | Ist erforderlich | Wert                                                                                                                                                                                                                           |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                  | Ja         | Der Name der zugehörigen Spalte.                                                                                                                                                                                           |
| **Type**                  | Ja         | Der Typ der zugehörigen Spalte.                                                                                                                                                                                           |
| **NULL-Werte zulässt**              | Nein          | **"True"** (Standardwert) oder **"false"** abhängig davon, ob die entsprechende Spalte einen null-Wert haben kann.                                                                                                                  |
| **DefaultValue**          | Nein          | Der Standardwert der zugehörigen Spalte.                                                                                                                                                                                  |
| **MaxLength**             | Nein          | Maximale Länge der zugehörigen Spalte.                                                                                                                                                                                 |
| **FixedLength**           | Nein          | **"True"** oder **"false"** abhängig davon, ob der entsprechende Spaltenwert als Zeichenfolge mit fester Länge gespeichert wird.                                                                                                              |
| **Genauigkeit**             | Nein          | Die Genauigkeit der zugehörigen Spalte.                                                                                                                                                                                      |
| **Scale** (Skalieren)                 | Nein          | Die Dezimalstellenanzahl der zugehörigen Spalte.                                                                                                                                                                                          |
| **Unicode**               | Nein          | **"True"** oder **"false"** abhängig davon, ob der entsprechende Spaltenwert als Unicode-Zeichenfolge gespeichert wird.                                                                                                                   |
| **Sortierung**             | Nein          | Eine Zeichenfolge, die die Sortierreihenfolge angibt, die in der Datenquelle verwendet werden soll.                                                                                                                                                   |
| **SRID**                  | Nein          | Räumliche Verweis Systembezeichner. Gilt nur für Eigenschaften des räumlichen Typen. Weitere Informationen finden Sie unter [SRID](http://en.wikipedia.org/wiki/SRID) und [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx). |
| **StoreGeneratedPattern** | Nein          | **Keine**, **Identität** (ist der entsprechende Spaltenwert eine Identität, die in der Datenbank generiert wird), oder **berechnet** (sofern der entsprechende Spaltenwert in der Datenbank generiert wird). Nicht gültig für RowType-Eigenschaften. |

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **Eigenschaft** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **EntityType** -Element mit zwei untergeordneten **Eigenschaft** Elemente:

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

## <a name="propertyref-element-ssdl"></a>PropertyRef-Element (SSDL)

Die **PropertyRef** Element in der Datenspeicherschema-Definitionssprache (SSDL) verweist auf eine Eigenschaft für ein EntityType-Element, um anzugeben, dass die Eigenschaft eine der folgenden Rollen ausführt:

-   Teil des Primärschlüssels der Tabelle sein, die die **EntityType** darstellt. Eine oder mehrere **PropertyRef** -Elemente können verwendet werden, um einen Primärschlüssel zu definieren. Weitere Informationen finden Sie in der Key-Element.
-   Sie ist das abhängige Ende oder das Prinzipalende einer referenziellen Einschränkung. Weitere Informationen finden Sie unter ReferentialConstraint-Element.

Die **PropertyRef** -Element kann nur die folgenden untergeordneten Elemente aufweisen:

-   Dokumentation (0 (null) oder ein Element)
-   Anmerkungselemente

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute, die angewendet werden können die **PropertyRef** Element.

| Attributname | Ist erforderlich | Wert                                |
|:---------------|:------------|:-------------------------------------|
| **Name**       | Ja         | Der Name der referenzierten Eigenschaft. |

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **PropertyRef** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für CSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **PropertyRef** Element so definieren Sie einen Primärschlüssel verweisen auf eine Eigenschaft, die auf definiert ist, verwendet eine **EntityType** Element.

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

## <a name="referentialconstraint-element-ssdl"></a>ReferentialConstraint-Element (SSDL)

Die **ReferentialConstraint** Element in der Datenspeicherschema-Definitionssprache (SSDL) stellt eine fremdschlüsseleinschränkung (auch Einschränkung der referenziellen Integrität genannt) in der zugrunde liegenden Datenbank. Das prinzipalende und das abhängige Ende der Einschränkung werden durch die Prinzipal- und die abhängigen untergeordneten Elemente angegeben. Spalten, die in den Dienstprinzipalnamen und die abhängigen enden beteiligt sind, werden mit PropertyRef-Elemente verwiesen.

Die **ReferentialConstraint** Element ist ein optionales untergeordnetes Element des Association-Elements. Wenn eine **ReferentialConstraint** Element wird nicht zum Zuordnen von foreign Key-Einschränkung, die im angegebenen verwendet die **Zuordnung** Element AssociationSetMapping-Element muss zu diesem Zweck verwendet werden.

Die **ReferentialConstraint** -Element kann die folgenden untergeordneten Elemente aufweisen:

-   Dokumentation (0 (null) oder ein Element)
-   Prinzipal (genau ein Element)
-   Abhängige (genau ein Element)
-   Anmerkungselemente (null oder mehr)

### <a name="applicable-attributes"></a>Anwendbare Attribute

Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **ReferentialConstraint** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **Zuordnung** -Element, das verwendet eine **ReferentialConstraint** Element, um die Spalten anzugeben, die Teilnahme an der **FK\_CustomerOrders**  foreign Key-Einschränkung:

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

## <a name="returntype-element-ssdl"></a>ReturnType-Element (SSDL)

Die **ReturnType** Element in der Datenspeicherschema-Definitionssprache (SSDL) gibt an, den Rückgabetyp für eine Funktion, die in definiert ist eine **Funktion** Element. Eine Funktion, der Rückgabetyp kann auch angegeben werden, mit einem **ReturnType** Attribut.

Der Rückgabetyp einer Funktion wird angegeben, mit der **Typ** Attribut oder das **ReturnType** Element.

Die **ReturnType** -Element kann die folgenden untergeordneten Elemente aufweisen:

- CollectionType (eins)  

> [!NOTE]
> Kann eine beliebige Anzahl von anmerkungsattribute (benutzerdefinierte XML-Attribute) angewendet werden, um die **ReturnType** Element. Benutzerdefinierte Attribute dürfen jedoch zu keinem XML-Namespace gehören, der für SSDL reserviert ist. Die vollqualifizierten Namen für zwei benutzerdefinierte Attribute dürfen nicht übereinstimmen.

### <a name="example"></a>Beispiel

Im folgenden Beispiel wird eine **Funktion** , die eine Auflistung von Zeilen zurückgibt.

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


## <a name="rowtype-element-ssdl"></a>RowType-Element (SSDL)

Ein **RowType** -Element der Datenspeicherschema-Definitionssprache (SSDL) definiert eine unbenannte Struktur als eine Rückgabe für eine Funktion, die im Speicher definiert.

Ein **RowType** Element ist das untergeordnete Element des **CollectionType** Element:

Ein **RowType** -Element kann die folgenden untergeordneten Elemente aufweisen:

- Eigenschaft (ein oder mehrere)  

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine Store-Funktion, verwendet eine **CollectionType** Element, um anzugeben, dass die Funktion eine Auflistung von Zeilen zurückgibt (gemäß den Angaben in der **RowType** Element).


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

## <a name="schema-element-ssdl"></a>Schema-Element (SSDL)

Die **Schema** Element in der Datenspeicherschema-Definitionssprache (SSDL) ist das Stammelement einer speichermodelldefinition. Es enthält Definitionen für die Objekte, Funktionen und Container, die zusammen ein Speichermodell bilden.

Die **Schema** -Element kann 0 (null) oder mehrere der folgenden untergeordneten Elemente enthalten:

-   Zuordnung
-   EntityType
-   EntityContainer
-   Funktion

Die **Schema** -Element verwendet die **Namespace** Attribut, um den Namespace für die Entität und des Zuordnungstyps der Objekte in einem Speichermodell definieren. Innerhalb eines Namespace müssen alle Objekte eine eindeutige Bezeichnung aufweisen.

Namespace für ein Speichermodell unterscheidet sich von den XML-Namespace die **Schema** Element. Namespace für ein Speichermodell (gemäß der **Namespace** Attribut) ist ein logischer Container für Entitätstypen und Zuordnungstypen. Der XML-Namespace (angegeben durch die **Xmlns** Attribut) des eine **Schema** Element ist der Standardnamespace für untergeordnete Elemente und Attribute des der **Schema** Element. XML-Namespaces im Format http://schemas.microsoft.com/ado/YYYY/MM/edm/ssdl (, wobei YYYY und MM ein Jahr und Monat darstellen) sind für SSDL reserviert. Benutzerdefinierte Elemente und Attribute können nicht in Namespaces mit diesem Format vorhanden sein.

### <a name="applicable-attributes"></a>Anwendbare Attribute

Die folgende Tabelle beschreibt die Attribute angewendet werden können, um die **Schema** Element.

| Attributname            | Ist erforderlich | Wert                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:--------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Namespace**             | Ja         | Der Namespace für das Speichermodell. Der Wert des der **Namespace** Attribut wird verwendet, um den vollqualifizierten Namen eines Typs zu bilden. Z. B. wenn ein **EntityType** mit dem Namen *Kunden* befindet sich im Simple.example.Model-Namespace, und klicken Sie dann auf den vollqualifizierten Namen des der **EntityType** ist ExampleModel.Store.Customer. <br/> Die folgenden Zeichenfolgen können nicht verwendet werden, als Wert für die **Namespace** Attribut: **System**, **vorübergehende**, oder **Edm**. Der Wert für die **Namespace** Attribut kann nicht übereinstimmen, den der Wert für die **Namespace** Attribut im CSDL-Schema-Element. |
| **Alias**                 | Nein          | Ein anstelle der Namespacebezeichnung verwendeter Bezeichner. Z. B. wenn ein **EntityType** mit dem Namen *Kunden* befindet sich im Simple.example.Model-Namespace und den Wert des der **Alias** Attribut *StorageModel*, dann kann StorageModel.Customer als vollqualifizierter Name des können Sie die **EntityType.**                                                                                                                                                                                                                                                                                    |
| **Anbieter**              | Ja         | Der Datenanbieter.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| **ProviderManifestToken** | Ja         | Ein Token, das dem Anbieter angibt, welches Anbietermanifest zurückgegeben werden soll. Für das Token ist kein Format definiert. Die für das Token möglichen Werte werden vom Anbieter definiert. Informationen zu SQL Server-Anbietermanifesttoken finden Sie in SqlClient für Entity Framework.                                                                                                                                                                                                                                                                                                                        |

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt eine **Schema** -Element mit einem **EntityContainer** Element, zwei **EntityType** -Elemente und ein **Zuordnung** Element.

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

## <a name="annotation-attributes"></a>Anmerkungsattribute

Anmerkungsattribute sind in der Datenspeicherschema-Definitionssprache (Store Schema Definition Language, SSDL) benutzerdefinierte XML-Attribute im Speichermodell, die zusätzliche Metadaten zu den Elementen im Speichermodell bereitstellen. Neben dem Vorhandensein einer gültigen XML-Struktur gelten für Anmerkungsattribute folgende Einschränkungen:

-   Anmerkungsattribute dürfen sich in keinem XML-Namespace befinden, der für SSDL reserviert ist.
-   Die vollqualifizierten Namen zweier Anmerkungsattribute dürfen nicht übereinstimmen.

Für ein angegebenes SSDL-Element kann mehr als ein Anmerkungsattribut übernommen werden. Metadaten in Anmerkungselementen kann zur Laufzeit zugegriffen werden, mithilfe von Klassen im Namespace System.Data.Metadata.Edm.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt ein EntityType-Element, das ein anmerkungsattribut übernommen verfügt die **"OrderID"** Eigenschaft. Das Beispiel zeigt außerdem ein Anmerkungselement, das hinzugefügt, die **EntityType** Element.

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

## <a name="annotation-elements-ssdl"></a>Anmerkungelemente (SSDL)

Anmerkungselemente sind in der Datenspeicherschema-Definitionssprache (Store Schema Definition Language, SSDL) benutzerdefinierte XML-Elemente im Speichermodell, die zusätzliche Metadaten zum Speichermodell bereitstellen. Neben dem Vorhandensein einer gültigen XML-Struktur gelten für Anmerkungselemente folgende Einschränkungen:

-   Anmerkungselemente dürfen sich in keinem XML-Namespace befinden, der für SSDL reserviert ist.
-   Die vollqualifizierten Namen zweier Anmerkungselemente dürfen nicht übereinstimmen.
-   Anmerkungselemente müssen nach allen anderen untergeordneten Elementen eines angegebenen SSDL-Elements angeordnet werden.

Mehrere Anmerkungselemente können untergeordnete Elemente eines angegebenen SSDL-Elements sein. Ab .NET Framework, Version 4, kann in Anmerkungselementen enthaltene Metadaten zur Laufzeit zugegriffen werden mithilfe von Klassen im Namespace System.Data.Metadata.Edm.

### <a name="example"></a>Beispiel

Das folgende Beispiel zeigt ein EntityType-Element, das ein Annotation-Element hat (**CustomElement**). Das Beispiel zeigt außerdem ein anmerkungsattribut übernommen werden, um die **"OrderID"** Eigenschaft.

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

## <a name="facets-ssdl"></a>Facets (SSDL)

Facets in Datenspeicherschema-Definitionssprache (SSDL) stellen Einschränkungen für Spaltentypen, die in Eigenschaftenelementen angegeben sind dar. Facets werden als XML-Attribute auf implementiert **Eigenschaft** Elemente.

In der folgenden Tabelle werden die in SSDL unterstützten Facets beschrieben:

| Facet           | Beschreibung                                                                                                                                                                                                                                                 |
|:----------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Sortierung**   | Gibt die bei Vergleich- und Sortiervorgängen zu verwendende Sortierreihenfolge für die Werte der Eigenschaft an.                                                                                                             |
| **FixedLength** | Gibt an, ob sich die Länge des Spaltenwerts ändern kann.                                                                                                                                                                                                  |
| **MaxLength**   | Gibt die maximale Länge des Spaltenwerts an.                                                                                                                                                                                                           |
| **Genauigkeit**   | Bei Eigenschaften vom Typ **Decimal**, gibt die Anzahl der Ziffern ein Eigenschaftswert verfügen kann. Bei Eigenschaften vom Typ **Zeit**, **"DateTime"**, und **DateTimeOffset**, gibt die Anzahl von Ziffern für den Bruchteil der Sekunden des Spaltenwerts. |
| **Scale** (Skalieren)       | Gibt die Anzahl der Dezimalstellen für den Spaltenwert an.                                                                                                                                                                      |
| **Unicode**     | Gibt an, ob der Spaltenwert als Unicode gespeichert wird.                                                                                                                                                                                                    |
