---
title: Beziehungen, Navigationseigenschaften und foreign key - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
caps.latest.revision: 3
ms.openlocfilehash: 0cc18ee8d1b9d1633535e4b8186ffc4c68daf32b
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120859"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a><span data-ttu-id="aa3f7-102">Beziehungen, Navigationseigenschaften und Fremdschlüssel</span><span class="sxs-lookup"><span data-stu-id="aa3f7-102">Relationships, navigation properties and foreign keys</span></span>
<span data-ttu-id="aa3f7-103">Dieses Thema bietet einen Überblick darüber, wie Beziehungen zwischen Entitäten von Entity Framework verwaltet.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-103">This topic gives an overview of how Entity Framework manages relationships between entities.</span></span> <span data-ttu-id="aa3f7-104">Außerdem erhalten hilfreiche Informationen zum Zuordnen und Bearbeiten von Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-104">It also gives some guidance on how to map and manipulate relationships.</span></span>

## <a name="relationships-in-ef"></a><span data-ttu-id="aa3f7-105">Beziehungen in EF</span><span class="sxs-lookup"><span data-stu-id="aa3f7-105">Relationships in EF</span></span>

<span data-ttu-id="aa3f7-106">In relationalen Datenbanken werden Beziehungen (auch als Zuordnungen bezeichnet) zwischen den Tabellen durch Fremdschlüssel definiert.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-106">In relational databases, relationships (also called associations) between tables are defined through foreign keys.</span></span> <span data-ttu-id="aa3f7-107">Ein Fremdschlüssel (FS) ist eine Spalte oder Kombination von Spalten, die verwendet wird eingerichtet und einen Link zwischen den Daten in zwei Tabellen erzwungen.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-107">A foreign key (FK) is a column or combination of columns that is used to establish and enforce a link between the data in two tables.</span></span> <span data-ttu-id="aa3f7-108">Es gibt im Allgemeinen drei Arten von Beziehungen: 1: 1, 1: n- und m: n.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-108">There are generally three types of relationships: one-to-one, one-to-many, and many-to-many.</span></span> <span data-ttu-id="aa3f7-109">In einer 1: n Beziehung wird die foreign Key für die Tabelle definiert, der das n-Ende der Beziehung darstellt.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-109">In a one-to-many relationship, the foreign key is defined on the table that represents the many end of the relationship.</span></span> <span data-ttu-id="aa3f7-110">Die m: n Beziehung umfasst, definieren eine dritte Tabelle (eine Verbindung oder Join-Tabelle), der Fremdschlüssel aus beiden verknüpften Tabellen, deren Primärschlüssel zusammengesetzt ist.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-110">The many-to-many relationship involves defining a third table (called a junction or join table), whose primary key is composed of the foreign keys from both related tables.</span></span> <span data-ttu-id="aa3f7-111">In einer 1: 1-Beziehung der Primärschlüssel fungiert außerdem als Fremdschlüssel, und es gibt keine separate Fremdschlüsselspalte für die beiden Tabellen.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-111">In a one-to-one relationship, the primary key acts additionally as a foreign key and there is no separate foreign key column for either table.</span></span>

<span data-ttu-id="aa3f7-112">Die folgende Abbildung zeigt zwei Tabellen, die Teilnahme an 1: n Beziehung.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-112">The following image shows two tables that participate in one-to-many relationship.</span></span> <span data-ttu-id="aa3f7-113">Die **Kurs** Tabelle ist die abhängigen Tabelle aus, da er enthält die **"DepartmentID"** Spalte, die sie verknüpft die **Abteilung** Tabelle.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-113">The **Course** table is the dependent table because it contains the **DepartmentID** column that links it to the **Department** table.</span></span>

!["Database2"](~/ef6/media/database2.png)

<span data-ttu-id="aa3f7-115">Im Entity Framework kann eine Entität auf andere Entitäten über eine Zuordnung oder Beziehung verknüpft werden.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-115">In Entity Framework, an entity can be related to other entities through an association or relationship.</span></span> <span data-ttu-id="aa3f7-116">Jede Beziehung enthält zwei Enden, die den Entitätstyp und die Multiplizität des Typs (eine, 0 (null) oder 1 oder viele) für die zwei Entitäten in dieser Beziehung beschreiben.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-116">Each relationship contains two ends that describe the entity type and the multiplicity of the type (one, zero-or-one, or many) for the two entities in that relationship.</span></span> <span data-ttu-id="aa3f7-117">Die Beziehung unterliegen möglicherweise eine referenzielle Einschränkung, die beschreibt, welches Ende der Beziehung die Prinzipalrolle ist und die abhängige Rolle ist.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-117">The relationship may be governed by a referential constraint, which describes which end in the relationship is a principal role and which is a dependent role.</span></span>

<span data-ttu-id="aa3f7-118">Navigationseigenschaften bieten eine Möglichkeit, eine Zuordnung zwischen zwei Entitätstypen zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-118">Navigation properties provide a way to navigate an association between two entity types.</span></span> <span data-ttu-id="aa3f7-119">Jedes Objekt kann für jede Beziehung, an der es beteiligt ist, über eine Navigationseigenschaft verfügen.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-119">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="aa3f7-120">Navigationseigenschaften können Sie zum Navigieren und Verwalten von Beziehungen in beide Richtungen, entweder ein Verweisobjekt zurückgegeben (wenn die Multiplizität entweder nacheinander oder 0 (null) oder 1) oder eine Sammlung (wenn die Multiplizität n ist).</span><span class="sxs-lookup"><span data-stu-id="aa3f7-120">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="aa3f7-121">Außerdem können Sie unidirektionale Navigation in diesem Fall definieren Sie die Navigationseigenschaft auf nur einer der Typen, die in der Beziehung beteiligt ist und nicht auf beide.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-121">You may also choose to have one-way navigation, in which case you define the navigation property on only one of the types that participates in the relationship and not on both.</span></span>

<span data-ttu-id="aa3f7-122">Es wird empfohlen, um Eigenschaften im Modell, die Fremdschlüssel in der Datenbank zugeordnet, sind.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-122">It is recommended to include properties in the model that map to foreign keys in the database.</span></span> <span data-ttu-id="aa3f7-123">Wenn Fremdschlüsseleigenschaften einbezogen wurden, können Sie durch das Ändern des Fremdschlüsselwerts für ein abhängiges Objekt eine Beziehung erstellen oder ändern.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-123">With foreign key properties included, you can create or change a relationship by modifying the foreign key value on a dependent object.</span></span> <span data-ttu-id="aa3f7-124">Diese Art von Zuordnung wird Fremdschlüsselzuordnung genannt.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-124">This kind of association is called a foreign key association.</span></span> <span data-ttu-id="aa3f7-125">Verwendung von Fremdschlüsseln ist noch wichtiger, bei der Arbeit mit getrennten Entitäten.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-125">Using foreign keys is even more essential when working with disconnected entities.</span></span> <span data-ttu-id="aa3f7-126">Beachten Sie, dass bei Arbeiten mit 1: 1- oder 1-zu-0... 1-Beziehungen, es gibt keine separate Fremdschlüsselspalte, die Primärschlüssel-Eigenschaft als Fremdschlüssel fungiert und ist immer im Modell enthalten.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-126">Note, that when working with 1-to-1 or 1-to-0..1 relationships, there is no separate foreign key column, the primary key property acts as the foreign key and is always included in the model.</span></span>

<span data-ttu-id="aa3f7-127">Wenn Fremdschlüsselspalten nicht im Modell enthalten sind, wird die Zuordnungsinformationen als unabhängiges Objekt verwaltet.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-127">When foreign key columns are not included in the model, the association information is managed as an independent object.</span></span> <span data-ttu-id="aa3f7-128">Beziehungen werden durch Objektverweise und nicht Fremdschlüsseleigenschaften verfolgt.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-128">Relationships are tracked through object references instead of foreign key properties.</span></span> <span data-ttu-id="aa3f7-129">Diese Art von Zuordnung wird aufgerufen, eine *unabhängige Zuordnung*.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-129">This type of association is called an *independent association*.</span></span> <span data-ttu-id="aa3f7-130">Die gängigste Methode zum Ändern einer *unabhängige Zuordnung* besteht darin, die Navigationseigenschaften zu ändern, die für jede Entität generiert werden, die in der Zuordnung teilnimmt.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-130">The most common way to modify an *independent association* is to modify the navigation properties that are generated for each entity that participates in the association.</span></span>

<span data-ttu-id="aa3f7-131">Sie können wahlweise einen oder beide Zuordnungstypen im Modell verwenden.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-131">You can choose to use one or both types of associations in your model.</span></span> <span data-ttu-id="aa3f7-132">Jedoch wenn Sie eine reine m: n Beziehung, die durch eine Jointabelle verbunden ist, die nur Fremdschlüssel enthält verfügen, wird EF eine unabhängige Zuordnung verwenden diese m: n Beziehung zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-132">However, if you have a pure many-to-many relationship that is connected by a join table that contains only foreign keys, the EF will use an independent association to manage such many-to-many relationship.</span></span>   

<span data-ttu-id="aa3f7-133">Die folgende Abbildung zeigt ein konzeptionelles Modell, das mit dem Entity Framework Designer erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-133">The following image shows a conceptual model that was created with the Entity Framework Designer.</span></span> <span data-ttu-id="aa3f7-134">Das Modell enthält zwei Entitäten, die in 1: n Beziehung beteiligt sind.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-134">The model contains two entities that participate in one-to-many relationship.</span></span> <span data-ttu-id="aa3f7-135">Beide Entitäten verfügen über Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-135">Both entities have navigation properties.</span></span> <span data-ttu-id="aa3f7-136">**Kurs** Depend Entität und die **"DepartmentID"** Fremdschlüsseleigenschaft definiert.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-136">**Course** is the depend entity and has the **DepartmentID** foreign key property defined.</span></span>

![RelationshipEFDesigner](~/ef6/media/relationshipefdesigner.png)

<span data-ttu-id="aa3f7-138">Der folgende Codeausschnitt zeigt das gleiche Modell, das mit Code First erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-138">The following code snippet shows the same model that was created with Code First.</span></span>

``` csharp
public class Course
{
  public int CourseID { get; set; }
  public string Title { get; set; }
  public int Credits { get; set; }
  public int DepartmentID { get; set; }
  public virtual Department Department { get; set; }
}

public class DepartmentID
{
   public Department()
   {
     this.Course = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a><span data-ttu-id="aa3f7-139">Konfigurieren oder die Zuordnung von Beziehungen</span><span class="sxs-lookup"><span data-stu-id="aa3f7-139">Configuring or mapping relationships</span></span>

<span data-ttu-id="aa3f7-140">Die restlichen auf dieser Seite wird beschrieben, wie Zugriff auf und Bearbeiten von Daten, die mithilfe von Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-140">The rest of this page covers how to access and manipulate data using relationships.</span></span> <span data-ttu-id="aa3f7-141">Informationen zum Einrichten der Beziehungen in Ihrem Modell finden Sie in den folgenden Seiten.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-141">For information on setting up relationships in your model, see the following pages.</span></span>

-   <span data-ttu-id="aa3f7-142">Um Beziehungen im Code First konfigurieren zu können, finden Sie unter [Datenanmerkungen](~/ef6/modeling/code-first/data-annotations.md) und [Fluent-API – Beziehungen](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="aa3f7-142">To configure relationships in Code First, see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API – Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>
-   <span data-ttu-id="aa3f7-143">Um die Beziehungen, die mit dem Entity Framework Designer konfigurieren zu können, finden Sie unter [Beziehungen mit dem EF Designer](~/ef6/modeling/designer/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="aa3f7-143">To configure relationships using the Entity Framework Designer, see [Relationships with the EF Designer](~/ef6/modeling/designer/relationships.md).</span></span>

## <a name="creating-and-modifying-relationships"></a><span data-ttu-id="aa3f7-144">Erstellen und Ändern von Beziehungen</span><span class="sxs-lookup"><span data-stu-id="aa3f7-144">Creating and modifying relationships</span></span>

<span data-ttu-id="aa3f7-145">In einem *fremdschlüsselzuordnung*, wenn Sie die Beziehung, die den Zustand eines abhängigen Objekts mit einem EntityState.Unchanged ändern, sich EntityState.Modified Status ändert.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-145">In a *foreign key association*, when you change the relationship, the state of a dependent object with an EntityState.Unchanged state changes to EntityState.Modified.</span></span> <span data-ttu-id="aa3f7-146">In einer unabhängigen Beziehung wird beim Ändern der Beziehung nicht den Zustand des abhängigen Objekts aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-146">In an independent relationship, changing the relationship does not update the state of the dependent object.</span></span>

<span data-ttu-id="aa3f7-147">Die folgenden Beispiele zeigen, wie Sie die Fremdschlüssel- und Navigationseigenschaften zu verwenden, um die verknüpften Objekte zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-147">The following examples show how to use the foreign key properties and navigation properties to associate the related objects.</span></span> <span data-ttu-id="aa3f7-148">Bei fremdschlüsselzuordnungen können Sie eine der Methoden ändern, erstellen oder Ändern von Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-148">With foreign key associations, you can use either method to change, create, or modify relationships.</span></span> <span data-ttu-id="aa3f7-149">Bei unabhängigen Zuordnungen können Sie die Fremdschlüsseleigenschaft nicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-149">With independent associations, you cannot use the foreign key property.</span></span>

-   <span data-ttu-id="aa3f7-150">Durch eine Fremdschlüsseleigenschaft wie im folgenden Beispiel wird einen neuen Wert zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-150">By assigning a new value to a foreign key property, as in the following example.</span></span>  
    ``` csharp
    course.DepartmentID = newCourse.DepartmentID;
    ```

-   <span data-ttu-id="aa3f7-151">Der folgende Code entfernt eine Beziehung durch Festlegen des Fremdschlüssels auf **null**.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-151">The following code removes a relationship by setting the foreign key to **null**.</span></span> <span data-ttu-id="aa3f7-152">Beachten Sie, dass die foreign Key-Eigenschaft auf NULL festlegbare sein muss.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-152">Note, that the foreign key property must be nullable.</span></span>  
    ``` csharp
    course.DepartmentID = null;
    ```  
    >[!NOTE]
    > <span data-ttu-id="aa3f7-153">Wenn der Verweis im hinzugefügten Zustand (in diesem Beispiel wird das Kurs-Objekt) ist, wird die verweisnavigationseigenschaft nicht synchronisiert werden mit den Schlüsselwerten eines neuen Objekts bis "SaveChanges" aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-153">If the reference is in the added state (in this example, the course object), the reference navigation property will not be synchronized with the key values of a new object until SaveChanges is called.</span></span> <span data-ttu-id="aa3f7-154">Es findet keine Synchronisierung statt, da der Objektkontext erst dann permanente Schlüssel für hinzugefügte Objekte enthält, wenn diese gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-154">Synchronization does not occur because the object context does not contain permanent keys for added objects until they are saved.</span></span> <span data-ttu-id="aa3f7-155">Wenn Sie neue Objekte vollständig synchronisiert werden, sobald Sie die Beziehung festgelegt haben müssen, verwenden Sie eines der folgenden methods.\*</span><span class="sxs-lookup"><span data-stu-id="aa3f7-155">If you must have new objects fully synchronized as soon as you set the relationship, use one of the following methods.\*</span></span>

-   <span data-ttu-id="aa3f7-156">Durch das Zuweisen einer Navigationseigenschaft zu einem neuen Objekt.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-156">By assigning a new object to a navigation property.</span></span> <span data-ttu-id="aa3f7-157">Der folgende Code erstellt eine Beziehung zwischen einem Kurs und einem `department`.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-157">The following code creates a relationship between a course and a `department`.</span></span> <span data-ttu-id="aa3f7-158">Wenn die Objekte an den Kontext angefügt werden die `course` wird ebenfalls hinzugefügt der `department.Courses` Sammlung und die entsprechende Fremdschlüsseleigenschaft Schlüsseleigenschaft auf die `course` -Objekts auf den Wert der Schlüsseleigenschaft der Abteilung festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-158">If the objects are attached to the context, the `course` is also added to the `department.Courses` collection, and the corresponding foreign key property on the `course` object is set to the key property value of the department.</span></span>  
    ``` csharp
    course.Department = department;
    ```

 -   <span data-ttu-id="aa3f7-159">Um die Beziehung löschen möchten, legen Sie die Navigationseigenschaft auf `null`.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-159">To delete the relationship, set the navigation property to `null`.</span></span> <span data-ttu-id="aa3f7-160">Wenn Sie mit Entity Framework, die auf .NET 4.0 basiert arbeiten, muss das verknüpfte Ende geladen werden, bevor Sie es auf null festlegen.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-160">If you are working with Entity Framework that is based on .NET 4.0, then the related end needs to be loaded before you set it to null.</span></span> <span data-ttu-id="aa3f7-161">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="aa3f7-161">For example:</span></span>  
    ``` chsarp
    context.Entry(course).Reference(c => c.Department).Load();  
    course.Department = null;
    ```  
    <span data-ttu-id="aa3f7-162">Ab Entity Framework 5.0, die auf .NET 4.5 basiert, können Sie die Beziehung auf null festlegen, ohne Sie zu das verknüpfte Ende zu laden.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-162">Starting with Entity Framework 5.0, that is based on .NET 4.5, you can set the relationship to null without loading the related end.</span></span> <span data-ttu-id="aa3f7-163">Sie können auch den aktuellen Wert auf null fest, mit dem folgenden Verfahren festlegen.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-163">You can also set the current value to null using the following method.</span></span>  
    ``` csharp
    context.Entry(course).Reference(c => c.Department).CurrentValue = null;
    ```

-   <span data-ttu-id="aa3f7-164">Durch das Löschen eines Objekts aus einer Entitätsauflistung oder das Hinzufügen eines Objekts zu einer Entitätsauflistung.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-164">By deleting or adding an object in an entity collection.</span></span> <span data-ttu-id="aa3f7-165">Sie können z. B. ein Objekt des Typs hinzufügen `Course` auf die `department.Courses` Auflistung.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-165">For example, you can add an object of type `Course` to the `department.Courses` collection.</span></span> <span data-ttu-id="aa3f7-166">Dieser Vorgang erstellt eine Beziehung zwischen einer bestimmten **Kurs** und einer bestimmten `department`.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-166">This operation creates a relationship between a particular **course** and a particular `department`.</span></span> <span data-ttu-id="aa3f7-167">Wenn die Objekte an den Kontext, den Verweis für die Abteilung und die Fremdschlüsseleigenschaft auf angefügt werden die **Kurs** Objekt wird an die entsprechende festgelegt `department`.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-167">If the objects are attached to the context, the department reference and the foreign key property on the **course** object will be set to the appropriate `department`.</span></span>  
    ``` csharp
    department.Courses.Add(newCourse);
    ```

- <span data-ttu-id="aa3f7-168">Mithilfe der `ChangeRelationshipState` Methode zum Ändern des Zustands der angegebenen Beziehung zwischen zwei Entitätsobjekten.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-168">By using the `ChangeRelationshipState` method to change the state of the specified relationship between two entity objects.</span></span> <span data-ttu-id="aa3f7-169">Diese Methode wird am häufigsten verwendet, bei der Arbeit mit N-schichtige Anwendungen und ein *unabhängige Zuordnung* (kann nicht mit einer fremdschlüsselzuordnung verwendet werden).</span><span class="sxs-lookup"><span data-stu-id="aa3f7-169">This method is most commonly used when working with N-Tier applications and an *independent association* (it cannot be used with a foreign key association).</span></span> <span data-ttu-id="aa3f7-170">Darüber hinaus mit dieser Methode müssen Sie löschen nach unten zum `ObjectContext`, wie im folgenden Beispiel gezeigt.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-170">Also, to use this method you must drop down to `ObjectContext`, as shown in the example below.</span></span>  
<span data-ttu-id="aa3f7-171">Im folgenden Beispiel besteht eine m: n Beziehung zwischen den Dozenten und Kurse zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-171">In the following example, there is a many-to-many relationship between Instructors and Courses.</span></span> <span data-ttu-id="aa3f7-172">Aufrufen der `ChangeRelationshipState` -Methode und übergeben die `EntityState.Added` Parameter können die `SchoolContext` wissen, dass eine Beziehung zwischen den beiden Objekten hinzugefügt wurde:</span><span class="sxs-lookup"><span data-stu-id="aa3f7-172">Calling the `ChangeRelationshipState` method and passing the `EntityState.Added` parameter, lets the `SchoolContext` know that a relationship has been added between the two objects:</span></span>

``` csharp

       ((IObjectContextAdapter)context).ObjectContext.
                 ObjectStateManager.
                  ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
```

    Note that if you are updating (not just adding) a relationship, you must delete the old relationship after adding the new one:

``` csharp
       ((IObjectContextAdapter)context).ObjectContext.
                  ObjectStateManager.
                  ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a><span data-ttu-id="aa3f7-173">Synchronisierung von Änderungen zwischen dem Fremdschlüssel und die Navigationseigenschaften</span><span class="sxs-lookup"><span data-stu-id="aa3f7-173">Synchronizing the changes between the foreign keys and navigation properties</span></span>

<span data-ttu-id="aa3f7-174">Wenn Sie die Beziehung zwischen den Objekten, die an den Kontext angefügt werden, mithilfe einer der oben beschriebenen Methoden ändern, muss Entity Framework, Fremdschlüssel, Verweise und Auflistungen synchronisieren. Entitätsframework verwaltet diese Synchronisierung (auch bekannt als Beziehung Reparaturmechanismus) automatisch für die POCO-Entitäten mit Proxys.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-174">When you change the relationship of the objects attached to the context by using one of the methods described above, Entity Framework needs to keep foreign keys, references, and collections in sync. Entity Framework automatically manages this synchronization (also known as relationship fix-up) for the POCO entities with proxies.</span></span> <span data-ttu-id="aa3f7-175">Weitere Informationen finden Sie unter [arbeiten mit Proxys in](~/ef6/fundamentals/proxies.md).</span><span class="sxs-lookup"><span data-stu-id="aa3f7-175">For more information, see [Working with Proxies](~/ef6/fundamentals/proxies.md).</span></span>

<span data-ttu-id="aa3f7-176">Wenn Sie POCO-Entitäten ohne Proxys verwenden, Sie müssen sicherstellen, dass die **DetectChanges** Methode wird aufgerufen, um die verknüpften Objekte im Kontext zu synchronisieren.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-176">If you are using POCO entities without proxies, you must make sure that the **DetectChanges** method is called to synchronize the related objects in the context.</span></span> <span data-ttu-id="aa3f7-177">Beachten Sie, dass, die die folgenden APIs automatisch lösen eine **DetectChanges** aufrufen.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-177">Note, that the following APIs automatically trigger a **DetectChanges** call.</span></span>

-   `DbSet.Add`
-   `DbSet.AddRange`
-   `DbSet.Remove`
-   `DbSet.RemoveRange`
-   `DbSet.Find`
-   `DbSet.Local`
-   `DbContext.SaveChanges`
-   `DbSet.Attach`
-   `DbContext.GetValidationErrors`
-   `DbContext.Entry`
-   `DbChangeTracker.Entries`
-   <span data-ttu-id="aa3f7-178">Ausführen einer LINQ-Abfrage für eine `DbSet`</span><span class="sxs-lookup"><span data-stu-id="aa3f7-178">Executing a LINQ query against a `DbSet`</span></span>

## <a name="loading-related-objects"></a><span data-ttu-id="aa3f7-179">Laden von verbundenen Objekten</span><span class="sxs-lookup"><span data-stu-id="aa3f7-179">Loading related objects</span></span>

<span data-ttu-id="aa3f7-180">Verwenden Sie im Entity Framework, die Sie am häufigsten verwenden die Navigationseigenschaften zum Laden von Entitäten, die auf die zurückgegebene Entität von der definierten Zuordnung beziehen.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-180">In Entity Framework you use most commonly use the navigation properties to load entities that are related to the returned entity by the defined association.</span></span> <span data-ttu-id="aa3f7-181">Weitere Informationen finden Sie unter [laden verbundener Objekte](~/ef6/querying/related-data.md).</span><span class="sxs-lookup"><span data-stu-id="aa3f7-181">For more information, see [Loading Related Objects](~/ef6/querying/related-data.md).</span></span>

> [!NOTE]
> <span data-ttu-id="aa3f7-182">Wenn Sie in einer Fremdschlüsselzuordnung ein verknüpftes Ende eines abhängigen Objekts laden, wird das verknüpfte Objekt auf der Grundlage des Fremdschlüsselwerts des abhängigen Elements geladen, der sich gerade im Arbeitsspeicher befindet:</span><span class="sxs-lookup"><span data-stu-id="aa3f7-182">In a foreign key association, when you load a related end of a dependent object, the related object will be loaded based on the foreign key value of the dependent that is currently in memory:</span></span>

``` csharp
    // Get the course where currently DepartmentID = 1.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

<span data-ttu-id="aa3f7-183">In einer unabhängigen Zuordnung wird das verknüpfte Ende eines abhängigen Objekts auf der Grundlage des Fremdschlüsselwerts abgefragt, der sich gerade in der Datenbank befindet.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-183">In an independent association, the related end of a dependent object is queried based on the foreign key value that is currently in the database.</span></span> <span data-ttu-id="aa3f7-184">Allerdings wird die Beziehung wurde geändert, und die Eigenschaft "Reference" auf dem abhängigen Objekts an ein anderes principal-Objekt, das in den Objektkontext, Entity Framework geladen wird versucht, erstellen Sie eine Beziehung als sie auf dem Client definiert.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-184">However, if the relationship was modified, and the reference property on the dependent object points to a different principal object that is loaded in the object context, Entity Framework will try to create a relationship as it is defined on the client.</span></span>

## <a name="managing-concurrency"></a><span data-ttu-id="aa3f7-185">Verwalten von Parallelität</span><span class="sxs-lookup"><span data-stu-id="aa3f7-185">Managing concurrency</span></span>

<span data-ttu-id="aa3f7-186">In sowohl Fremdschlüssel als auch unabhängigen Zuordnungen basieren parallelitätsüberprüfungen auf den Entitätsschlüsseln und anderen Entitätseigenschaften, die im Modell definiert sind.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-186">In both foreign key and independent associations, concurrency checks are based on the entity keys and other entity properties that are defined in the model.</span></span> <span data-ttu-id="aa3f7-187">Wenn Sie dem EF Designer zum Erstellen eines Modells verwenden, legen Sie die `ConcurrencyMode` -Attribut auf **behoben** um anzugeben, dass die Eigenschaft für die Parallelität überprüft werden sollen.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-187">When using the EF Designer to create a model, set the `ConcurrencyMode` attribute to **fixed** to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="aa3f7-188">Code First zum Definieren eines Modells verwenden Sie die `ConcurrencyCheck` Anmerkung zu Eigenschaften, die für die Parallelität, die überprüft werden soll.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-188">When using Code First to define a model, use the `ConcurrencyCheck` annotation on properties that you want to be checked for concurrency.</span></span> <span data-ttu-id="aa3f7-189">Bei der Arbeit mit Code First können Sie auch die `TimeStamp` Anmerkung, um anzugeben, dass die Eigenschaft für die Parallelität überprüft werden sollen.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-189">When working with Code First you can also use the `TimeStamp` annotation to specify that the property should be checked for concurrency.</span></span> <span data-ttu-id="aa3f7-190">Sie können nur eine Timestamp-Eigenschaft in einer bestimmten Klasse verwenden.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-190">You can have only one timestamp property in a given class.</span></span> <span data-ttu-id="aa3f7-191">Code ordnet zuerst diese Eigenschaft einen NULL-Feld in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-191">Code First maps this property to a non-nullable field in the database.</span></span>

<span data-ttu-id="aa3f7-192">Es wird empfohlen, dass Sie immer die fremdschlüsselzuordnung verwenden, bei der Verwendung von Entitäten, die parallelitätsüberprüfung und der Lösung beteiligt sind.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-192">We recommend that you always use the foreign key association when working with entities that participate in concurrency checking and resolution.</span></span>

<span data-ttu-id="aa3f7-193">Weitere Informationen finden Sie unter [Behandlung von Nebenläufigkeitskonflikten](~/ef6/saving/concurrency.md).</span><span class="sxs-lookup"><span data-stu-id="aa3f7-193">For more information, see [Handling Concurrency Conflicts](~/ef6/saving/concurrency.md).</span></span>

## <a name="working-with-overlapping-keys"></a><span data-ttu-id="aa3f7-194">Arbeiten mit überlappenden Schlüsseln</span><span class="sxs-lookup"><span data-stu-id="aa3f7-194">Working with overlapping Keys</span></span>

<span data-ttu-id="aa3f7-195">Überlappende Schlüssel sind zusammengesetzte Schlüssel, wobei einige Eigenschaften eines Schlüssels auch Teil eines anderen Schlüssels der Entität sind.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-195">Overlapping keys are composite keys where some properties in the key are also part of another key in the entity.</span></span> <span data-ttu-id="aa3f7-196">Unabhängige Zuordnungen können keine überlappenden Schlüssel enthalten.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-196">You cannot have an overlapping key in an independent association.</span></span> <span data-ttu-id="aa3f7-197">Um eine Fremdschlüsselzuordnung zu ändern, die überlappende Schlüssel enthält, empfiehlt es sich, die Fremdschlüsselwerte zu ändern, statt die Objektverweise zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="aa3f7-197">To change a foreign key association that includes overlapping keys, we recommend that you modify the foreign key values instead of using the object references.</span></span>
