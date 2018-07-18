---
title: Fluent-API - Beziehungen - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: fd73b4f8-16d5-40f1-9640-885ceafe67a1
caps.latest.revision: 3
ms.openlocfilehash: 7437b395fbed07dcb8c93cd8418317611e14a60b
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121102"
---
# <a name="fluent-api---relationships"></a>Fluent-API – Beziehungen
> [!NOTE]
> Diese Seite enthält Informationen zum Einrichten der Beziehungen in Ihrem Code First-Modell mithilfe der fluent-API. Allgemeine Informationen zu Beziehungen in Entity Framework und das Zugreifen auf und Bearbeiten von Daten mithilfe von Beziehungen, finden Sie unter [Beziehungen und Navigationseigenschaften](~/ef6/fundamentals/relationships.md).  

Bei der Arbeit mit Code First definieren Sie das Modell durch die Definition der Domäne CLR-Klassen. Standardmäßig verwendet Entity Framework Code First-Konventionen, die Klassen mit dem Datenbankschema zuzuordnen. Wenn Sie die Code First Benennungskonventionen verwenden, können in den meisten Fällen Sie verlassen auf Code First zum Einrichten der Beziehungen zwischen den Tabellen basierend auf den Fremdschlüssel und die Navigationseigenschaften, die Sie in den Klassen definieren. Wenn Sie nicht die Konventionen beim Definieren von Klassen unterliegen, oder wenn Sie verändern möchten die Konventionen arbeiten, können Sie die fluent-API oder datenanmerkungen verwenden, Ihre Klassen zu konfigurieren, damit der Code First die Beziehungen zwischen Tabellen zugeordnet werden können.  

## <a name="introduction"></a>Einführung  

Wenn Sie eine Beziehung mit der fluent-API zu konfigurieren, beginnen Sie mit der EntityTypeConfiguration-Instanz, und Sie dann verwenden Sie die HasRequired HasOptional oder HasMany-Methode, um den Typ der Beziehung anzugeben, die diese Entität teilnimmt. Die HasRequired und HasOptional Methoden nehmen einen Lambda-Ausdruck, der eine Verweis-Navigationseigenschaft darstellt. Die HasMany-Methode nimmt es sich um einen Lambda-Ausdruck, der eine auflistungsnavigationseigenschaft darstellt. Sie können eine inverse Navigationseigenschaft klicken Sie dann mithilfe der Methoden WithRequired WithOptional und WithMany konfigurieren. Diese Methoden verfügen über Überladungen, die akzeptieren keine Argumente und können verwendet werden, um die Kardinalität mit unidirektionalen Navigationen angeben.  

Sie können dann Fremdschlüsseleigenschaften konfigurieren, mit die HasForeignKey-Methode. Diese Methode verwendet einen Lambda-Ausdruck ein, der die darstellt, die als Fremdschlüssel verwendet werden.  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a>Konfigurieren eine erforderlich, um – optionale Beziehung (One-– 0 (null) oder-1)  

Im folgenden Beispiel wird eine 1: 0 (null)-oder-1-Beziehung. Die "OfficeAssignment" verfügt über die InstructorID-Eigenschaft, die einen Primärschlüssel und ein Fremdschlüssel, aus, da der Name der Eigenschaft nicht die Konvention folgt, die die HasKey-Methode verwendet wird, so konfigurieren Sie den primären Schlüssel.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a>Konfigurieren eine Beziehung, in dem beide Enden (eins zu eins) erforderlich sind  

In den meisten Fällen kann Entity Framework ableiten, welche Art der abhängigen und der Prinzipal in einer Beziehung. Wenn beide Enden der Beziehung erforderlich sind, oder beide Seiten sind optionale Entity Framework können jedoch nicht dem abhängigen und der Prinzipal identifizieren. Wenn beide Enden der Beziehung erforderlich sind, verwenden Sie nach der Methode HasRequired WithRequiredPrincipal oder WithRequiredDependent an. Wenn beide Enden der Beziehung optional sind, verwenden Sie WithOptionalPrincipal oder WithOptionalDependent, nach der HasOptional-Methode.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a>Konfigurieren eine m: N Beziehung  

Der folgende Code konfiguriert eine m: n Beziehung zwischen den Kurs und "Instructor". Im folgenden Beispiel werden die Standard-Code First-Konventionen zum Erstellen einer Jointabelle verwendet. Daher wird die Tabelle CourseInstructor mit Course_CourseID und Instructor_InstructorID Spalten erstellt.  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

Wenn Sie den Namen der Jointabelle und die Namen der Spalten in der Tabelle angeben möchten, müssen Sie zusätzliche Konfigurationsschritte ausführen, mit der Map-Methode. Der folgende Code generiert die CourseInstructor-Tabelle mit den Spalten "CourseID" und "InstructorID.  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
    .Map(m =>
    {
        m.ToTable("CourseInstructor");
        m.MapLeftKey("CourseID");
        m.MapRightKey("InstructorID");
    });
```  

## <a name="configuring-a-relationship-with-one-navigation-property"></a>Konfigurieren eine Beziehung mit einer Navigationseigenschaft  

Ein unidirektionales (auch als unidirektional) Beziehung ist, wenn eine Navigationseigenschaft auf nur eine Beziehung enden und nicht auf beide definiert ist. Gemäß der Konvention interpretiert Code First immer als 1: n eine unidirektionale Beziehung. Wenn Sie möchten eine direkte Beziehung zwischen "Instructor" und "OfficeAssignment", wenn Sie eine Navigationseigenschaft für nur den Typ "Instructor" verfügen, müssen Sie z. B. die fluent-API verwenden, um diese Beziehung zu konfigurieren.  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a>Aktivieren der Löschweitergabe  

Sie können die Löschweitergabe auf eine Beziehung konfigurieren, mit der WillCascadeOnDelete-Methode. Ist ein Fremdschlüssel für die abhängige Entität nicht NULL-Werte zulässt, legt dann Code First Löschweitergabe für die Beziehung. Wenn ein Fremdschlüssel in der abhängigen Entität NULL-Werte zulässt ist, Code First Löschweitergabe nicht auf der Beziehung fest, und wenn der Prinzipal gelöscht wird die foreign Key wird festgelegt auf Null.  

Sie können diese Cascade Delete-Konventionen mit entfernen:  

modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>)  
modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>)  

Der folgende Code konfiguriert die Beziehung als erforderlich sein, und anschließend deaktiviert er Löschweitergabe.  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a>Konfigurieren einen zusammengesetzten Foreign Key  

Wenn Sie der primäre Schlüssel für den Typ der Abteilung "DepartmentID" und der Namenseigenschaften zusammensetzte, würden Sie den primären Schlüssel für die Abteilung und die foreign Key für die Kurs-Typen wie folgt konfigurieren:  

``` csharp
// Composite primary key
modelBuilder.Entity<Department>()
.HasKey(d => new { d.DepartmentID, d.Name });

// Composite foreign key
modelBuilder.Entity<Course>()  
    .HasRequired(c => c.Department)  
    .WithMany(d => d.Courses)
    .HasForeignKey(d => new { d.DepartmentID, d.DepartmentName });
```  

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Umbenennen von einem Fremdschlüssel, die nicht im Modell definiert ist  

Wenn Sie keinen Fremdschlüssel für die CLR-Typ definieren, aber welchen Namen verfügen sollte in der Datenbank angeben möchten, führen Sie folgende Schritte aus:  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a>Konfigurieren einen Foreign Key-Namen, die nicht die erste Code-Konvention folgt  

Wenn die Fremdschlüsseleigenschaft für die Kurs-Klasse SomeDepartmentID anstelle von "DepartmentID" aufgerufen wurde, müssten Sie die folgenden Schritte zum angeben, dass Sie SomeDepartmentID der Fremdschlüssel sein soll:  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a>In den Beispielen verwendete Modell  

Der folgende Code First-Modell wird für die Beispiele auf dieser Seite verwendet.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.ModelConfiguration.Conventions;
// add a reference to System.ComponentModel.DataAnnotations DLL
using System.ComponentModel.DataAnnotations;
using System.Collections.Generic;
using System;

public class SchoolEntities : DbContext
{
    public DbSet<Course> Courses { get; set; }
    public DbSet<Department> Departments { get; set; }
    public DbSet<Instructor> Instructors { get; set; }
    public DbSet<OfficeAssignment> OfficeAssignments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention then the generated tables will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}

public class Department
{
    public Department()
    {
        this.Courses = new HashSet<Course>();
    }
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }
    public decimal Budget { get; set; }
    public System.DateTime StartDate { get; set; }
    public int? Administrator { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; private set; }
}

public class Course
{
    public Course()
    {
        this.Instructors = new HashSet<Instructor>();
    }
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
    public virtual ICollection<Instructor> Instructors { get; private set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

public partial class OnsiteCourse : Course
{
    public OnsiteCourse()
    {
        Details = new Details();
    }

    public Details Details { get; set; }
}

public class Details
{
    public System.DateTime Time { get; set; }
    public string Location { get; set; }
    public string Days { get; set; }
}

public class Instructor
{
    public Instructor()
    {
        this.Courses = new List<Course>();
    }

    // Primary key
    public int InstructorID { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
    public System.DateTime HireDate { get; set; }

    // Navigation properties
    public virtual ICollection<Course> Courses { get; private set; }
}

public class OfficeAssignment
{
    // Specifying InstructorID as a primary
    [Key()]
    public Int32 InstructorID { get; set; }

    public string Location { get; set; }

    // When Entity Framework sees Timestamp attribute
    // it configures ConcurrencyCheck and DatabaseGeneratedPattern=Computed.
    [Timestamp]
    public Byte[] Timestamp { get; set; }

    // Navigation property
    public virtual Instructor Instructor { get; set; }
}
```  
