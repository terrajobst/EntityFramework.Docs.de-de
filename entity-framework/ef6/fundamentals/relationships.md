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
# <a name="relationships-navigation-properties-and-foreign-keys"></a>Beziehungen, Navigationseigenschaften und Fremdschlüssel
Dieses Thema bietet einen Überblick darüber, wie Beziehungen zwischen Entitäten von Entity Framework verwaltet. Außerdem erhalten hilfreiche Informationen zum Zuordnen und Bearbeiten von Beziehungen.

## <a name="relationships-in-ef"></a>Beziehungen in EF

In relationalen Datenbanken werden Beziehungen (auch als Zuordnungen bezeichnet) zwischen den Tabellen durch Fremdschlüssel definiert. Ein Fremdschlüssel (FS) ist eine Spalte oder Kombination von Spalten, die verwendet wird eingerichtet und einen Link zwischen den Daten in zwei Tabellen erzwungen. Es gibt im Allgemeinen drei Arten von Beziehungen: 1: 1, 1: n- und m: n. In einer 1: n Beziehung wird die foreign Key für die Tabelle definiert, der das n-Ende der Beziehung darstellt. Die m: n Beziehung umfasst, definieren eine dritte Tabelle (eine Verbindung oder Join-Tabelle), der Fremdschlüssel aus beiden verknüpften Tabellen, deren Primärschlüssel zusammengesetzt ist. In einer 1: 1-Beziehung der Primärschlüssel fungiert außerdem als Fremdschlüssel, und es gibt keine separate Fremdschlüsselspalte für die beiden Tabellen.

Die folgende Abbildung zeigt zwei Tabellen, die Teilnahme an 1: n Beziehung. Die **Kurs** Tabelle ist die abhängigen Tabelle aus, da er enthält die **"DepartmentID"** Spalte, die sie verknüpft die **Abteilung** Tabelle.

!["Database2"](~/ef6/media/database2.png)

Im Entity Framework kann eine Entität auf andere Entitäten über eine Zuordnung oder Beziehung verknüpft werden. Jede Beziehung enthält zwei Enden, die den Entitätstyp und die Multiplizität des Typs (eine, 0 (null) oder 1 oder viele) für die zwei Entitäten in dieser Beziehung beschreiben. Die Beziehung unterliegen möglicherweise eine referenzielle Einschränkung, die beschreibt, welches Ende der Beziehung die Prinzipalrolle ist und die abhängige Rolle ist.

Navigationseigenschaften bieten eine Möglichkeit, eine Zuordnung zwischen zwei Entitätstypen zu navigieren. Jedes Objekt kann für jede Beziehung, an der es beteiligt ist, über eine Navigationseigenschaft verfügen. Navigationseigenschaften können Sie zum Navigieren und Verwalten von Beziehungen in beide Richtungen, entweder ein Verweisobjekt zurückgegeben (wenn die Multiplizität entweder nacheinander oder 0 (null) oder 1) oder eine Sammlung (wenn die Multiplizität n ist). Außerdem können Sie unidirektionale Navigation in diesem Fall definieren Sie die Navigationseigenschaft auf nur einer der Typen, die in der Beziehung beteiligt ist und nicht auf beide.

Es wird empfohlen, um Eigenschaften im Modell, die Fremdschlüssel in der Datenbank zugeordnet, sind. Wenn Fremdschlüsseleigenschaften einbezogen wurden, können Sie durch das Ändern des Fremdschlüsselwerts für ein abhängiges Objekt eine Beziehung erstellen oder ändern. Diese Art von Zuordnung wird Fremdschlüsselzuordnung genannt. Verwendung von Fremdschlüsseln ist noch wichtiger, bei der Arbeit mit getrennten Entitäten. Beachten Sie, dass bei Arbeiten mit 1: 1- oder 1-zu-0... 1-Beziehungen, es gibt keine separate Fremdschlüsselspalte, die Primärschlüssel-Eigenschaft als Fremdschlüssel fungiert und ist immer im Modell enthalten.

Wenn Fremdschlüsselspalten nicht im Modell enthalten sind, wird die Zuordnungsinformationen als unabhängiges Objekt verwaltet. Beziehungen werden durch Objektverweise und nicht Fremdschlüsseleigenschaften verfolgt. Diese Art von Zuordnung wird aufgerufen, eine *unabhängige Zuordnung*. Die gängigste Methode zum Ändern einer *unabhängige Zuordnung* besteht darin, die Navigationseigenschaften zu ändern, die für jede Entität generiert werden, die in der Zuordnung teilnimmt.

Sie können wahlweise einen oder beide Zuordnungstypen im Modell verwenden. Jedoch wenn Sie eine reine m: n Beziehung, die durch eine Jointabelle verbunden ist, die nur Fremdschlüssel enthält verfügen, wird EF eine unabhängige Zuordnung verwenden diese m: n Beziehung zu verwalten.   

Die folgende Abbildung zeigt ein konzeptionelles Modell, das mit dem Entity Framework Designer erstellt wurde. Das Modell enthält zwei Entitäten, die in 1: n Beziehung beteiligt sind. Beide Entitäten verfügen über Navigationseigenschaften. **Kurs** Depend Entität und die **"DepartmentID"** Fremdschlüsseleigenschaft definiert.

![RelationshipEFDesigner](~/ef6/media/relationshipefdesigner.png)

Der folgende Codeausschnitt zeigt das gleiche Modell, das mit Code First erstellt wurde.

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

## <a name="configuring-or-mapping-relationships"></a>Konfigurieren oder die Zuordnung von Beziehungen

Die restlichen auf dieser Seite wird beschrieben, wie Zugriff auf und Bearbeiten von Daten, die mithilfe von Beziehungen. Informationen zum Einrichten der Beziehungen in Ihrem Modell finden Sie in den folgenden Seiten.

-   Um Beziehungen im Code First konfigurieren zu können, finden Sie unter [Datenanmerkungen](~/ef6/modeling/code-first/data-annotations.md) und [Fluent-API – Beziehungen](~/ef6/modeling/code-first/fluent/relationships.md).
-   Um die Beziehungen, die mit dem Entity Framework Designer konfigurieren zu können, finden Sie unter [Beziehungen mit dem EF Designer](~/ef6/modeling/designer/relationships.md).

## <a name="creating-and-modifying-relationships"></a>Erstellen und Ändern von Beziehungen

In einem *fremdschlüsselzuordnung*, wenn Sie die Beziehung, die den Zustand eines abhängigen Objekts mit einem EntityState.Unchanged ändern, sich EntityState.Modified Status ändert. In einer unabhängigen Beziehung wird beim Ändern der Beziehung nicht den Zustand des abhängigen Objekts aktualisiert werden.

Die folgenden Beispiele zeigen, wie Sie die Fremdschlüssel- und Navigationseigenschaften zu verwenden, um die verknüpften Objekte zuzuordnen. Bei fremdschlüsselzuordnungen können Sie eine der Methoden ändern, erstellen oder Ändern von Beziehungen. Bei unabhängigen Zuordnungen können Sie die Fremdschlüsseleigenschaft nicht verwenden.

-   Durch eine Fremdschlüsseleigenschaft wie im folgenden Beispiel wird einen neuen Wert zugewiesen.  
    ``` csharp
    course.DepartmentID = newCourse.DepartmentID;
    ```

-   Der folgende Code entfernt eine Beziehung durch Festlegen des Fremdschlüssels auf **null**. Beachten Sie, dass die foreign Key-Eigenschaft auf NULL festlegbare sein muss.  
    ``` csharp
    course.DepartmentID = null;
    ```  
    >[!NOTE]
    > Wenn der Verweis im hinzugefügten Zustand (in diesem Beispiel wird das Kurs-Objekt) ist, wird die verweisnavigationseigenschaft nicht synchronisiert werden mit den Schlüsselwerten eines neuen Objekts bis "SaveChanges" aufgerufen wird. Es findet keine Synchronisierung statt, da der Objektkontext erst dann permanente Schlüssel für hinzugefügte Objekte enthält, wenn diese gespeichert werden. Wenn Sie neue Objekte vollständig synchronisiert werden, sobald Sie die Beziehung festgelegt haben müssen, verwenden Sie eines der folgenden methods.*

-   Durch das Zuweisen einer Navigationseigenschaft zu einem neuen Objekt. Der folgende Code erstellt eine Beziehung zwischen einem Kurs und einem `department`. Wenn die Objekte an den Kontext angefügt werden die `course` wird ebenfalls hinzugefügt der `department.Courses` Sammlung und die entsprechende Fremdschlüsseleigenschaft Schlüsseleigenschaft auf die `course` -Objekts auf den Wert der Schlüsseleigenschaft der Abteilung festgelegt wird.  
    ``` csharp
    course.Department = department;
    ```

 -   Um die Beziehung löschen möchten, legen Sie die Navigationseigenschaft auf `null`. Wenn Sie mit Entity Framework, die auf .NET 4.0 basiert arbeiten, muss das verknüpfte Ende geladen werden, bevor Sie es auf null festlegen. Zum Beispiel:  
    ``` chsarp
    context.Entry(course).Reference(c => c.Department).Load();  
    course.Department = null;
    ```  
    Ab Entity Framework 5.0, die auf .NET 4.5 basiert, können Sie die Beziehung auf null festlegen, ohne Sie zu das verknüpfte Ende zu laden. Sie können auch den aktuellen Wert auf null fest, mit dem folgenden Verfahren festlegen.  
    ``` csharp
    context.Entry(course).Reference(c => c.Department).CurrentValue = null;
    ```

-   Durch das Löschen eines Objekts aus einer Entitätsauflistung oder das Hinzufügen eines Objekts zu einer Entitätsauflistung. Sie können z. B. ein Objekt des Typs hinzufügen `Course` auf die `department.Courses` Auflistung. Dieser Vorgang erstellt eine Beziehung zwischen einer bestimmten **Kurs** und einer bestimmten `department`. Wenn die Objekte an den Kontext, den Verweis für die Abteilung und die Fremdschlüsseleigenschaft auf angefügt werden die **Kurs** Objekt wird an die entsprechende festgelegt `department`.  
    ``` csharp
    department.Courses.Add(newCourse);
    ```

- Mithilfe der `ChangeRelationshipState` Methode zum Ändern des Zustands der angegebenen Beziehung zwischen zwei Entitätsobjekten. Diese Methode wird am häufigsten verwendet, bei der Arbeit mit N-schichtige Anwendungen und ein *unabhängige Zuordnung* (kann nicht mit einer fremdschlüsselzuordnung verwendet werden). Darüber hinaus mit dieser Methode müssen Sie löschen nach unten zum `ObjectContext`, wie im folgenden Beispiel gezeigt.  
Im folgenden Beispiel besteht eine m: n Beziehung zwischen den Dozenten und Kurse zur Verfügung. Aufrufen der `ChangeRelationshipState` -Methode und übergeben die `EntityState.Added` Parameter können die `SchoolContext` wissen, dass eine Beziehung zwischen den beiden Objekten hinzugefügt wurde:

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

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a>Synchronisierung von Änderungen zwischen dem Fremdschlüssel und die Navigationseigenschaften

Wenn Sie die Beziehung zwischen den Objekten, die an den Kontext angefügt werden, mithilfe einer der oben beschriebenen Methoden ändern, muss Entity Framework, Fremdschlüssel, Verweise und Auflistungen synchronisieren. Entitätsframework verwaltet diese Synchronisierung (auch bekannt als Beziehung Reparaturmechanismus) automatisch für die POCO-Entitäten mit Proxys. Weitere Informationen finden Sie unter [arbeiten mit Proxys in](~/ef6/fundamentals/proxies.md).

Wenn Sie POCO-Entitäten ohne Proxys verwenden, Sie müssen sicherstellen, dass die **DetectChanges** Methode wird aufgerufen, um die verknüpften Objekte im Kontext zu synchronisieren. Beachten Sie, dass, die die folgenden APIs automatisch lösen eine **DetectChanges** aufrufen.

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
-   Ausführen einer LINQ-Abfrage für eine `DbSet`

## <a name="loading-related-objects"></a>Laden von verbundenen Objekten

Verwenden Sie im Entity Framework, die Sie am häufigsten verwenden die Navigationseigenschaften zum Laden von Entitäten, die auf die zurückgegebene Entität von der definierten Zuordnung beziehen. Weitere Informationen finden Sie unter [laden verbundener Objekte](~/ef6/querying/related-data.md).

> [!NOTE]
> Wenn Sie in einer Fremdschlüsselzuordnung ein verknüpftes Ende eines abhängigen Objekts laden, wird das verknüpfte Objekt auf der Grundlage des Fremdschlüsselwerts des abhängigen Elements geladen, der sich gerade im Arbeitsspeicher befindet:

``` csharp
    // Get the course where currently DepartmentID = 1.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

In einer unabhängigen Zuordnung wird das verknüpfte Ende eines abhängigen Objekts auf der Grundlage des Fremdschlüsselwerts abgefragt, der sich gerade in der Datenbank befindet. Allerdings wird die Beziehung wurde geändert, und die Eigenschaft "Reference" auf dem abhängigen Objekts an ein anderes principal-Objekt, das in den Objektkontext, Entity Framework geladen wird versucht, erstellen Sie eine Beziehung als sie auf dem Client definiert.

## <a name="managing-concurrency"></a>Verwalten von Parallelität

In sowohl Fremdschlüssel als auch unabhängigen Zuordnungen basieren parallelitätsüberprüfungen auf den Entitätsschlüsseln und anderen Entitätseigenschaften, die im Modell definiert sind. Wenn Sie dem EF Designer zum Erstellen eines Modells verwenden, legen Sie die `ConcurrencyMode` -Attribut auf **behoben** um anzugeben, dass die Eigenschaft für die Parallelität überprüft werden sollen. Code First zum Definieren eines Modells verwenden Sie die `ConcurrencyCheck` Anmerkung zu Eigenschaften, die für die Parallelität, die überprüft werden soll. Bei der Arbeit mit Code First können Sie auch die `TimeStamp` Anmerkung, um anzugeben, dass die Eigenschaft für die Parallelität überprüft werden sollen. Sie können nur eine Timestamp-Eigenschaft in einer bestimmten Klasse verwenden. Code ordnet zuerst diese Eigenschaft einen NULL-Feld in der Datenbank.

Es wird empfohlen, dass Sie immer die fremdschlüsselzuordnung verwenden, bei der Verwendung von Entitäten, die parallelitätsüberprüfung und der Lösung beteiligt sind.

Weitere Informationen finden Sie unter [Behandlung von Nebenläufigkeitskonflikten](~/ef6/saving/concurrency.md).

## <a name="working-with-overlapping-keys"></a>Arbeiten mit überlappenden Schlüsseln

Überlappende Schlüssel sind zusammengesetzte Schlüssel, wobei einige Eigenschaften eines Schlüssels auch Teil eines anderen Schlüssels der Entität sind. Unabhängige Zuordnungen können keine überlappenden Schlüssel enthalten. Um eine Fremdschlüsselzuordnung zu ändern, die überlappende Schlüssel enthält, empfiehlt es sich, die Fremdschlüsselwerte zu ändern, statt die Objektverweise zu verwenden.
