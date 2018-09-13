---
title: Fluent-API - Beziehungen - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: fd73b4f8-16d5-40f1-9640-885ceafe67a1
ms.openlocfilehash: 05f282c02699f8bf3c71197ac5e01000f1855917
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490466"
---
# <a name="fluent-api---relationships"></a><span data-ttu-id="a7dd3-102">Fluent-API – Beziehungen</span><span class="sxs-lookup"><span data-stu-id="a7dd3-102">Fluent API - Relationships</span></span>
> [!NOTE]
> <span data-ttu-id="a7dd3-103">Diese Seite enthält Informationen zum Einrichten der Beziehungen in Ihrem Code First-Modell mithilfe der fluent-API.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-103">This page provides information about setting up relationships in your Code First model using the fluent API.</span></span> <span data-ttu-id="a7dd3-104">Allgemeine Informationen zu Beziehungen in Entity Framework und das Zugreifen auf und Bearbeiten von Daten mithilfe von Beziehungen, finden Sie unter [Beziehungen und Navigationseigenschaften](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="a7dd3-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>  

<span data-ttu-id="a7dd3-105">Bei der Arbeit mit Code First definieren Sie das Modell durch die Definition der Domäne CLR-Klassen.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-105">When working with Code First, you define your model by defining your domain CLR classes.</span></span> <span data-ttu-id="a7dd3-106">Standardmäßig verwendet Entity Framework Code First-Konventionen, die Klassen mit dem Datenbankschema zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-106">By default, Entity Framework uses the Code First conventions to map your classes to the database schema.</span></span> <span data-ttu-id="a7dd3-107">Wenn Sie die Code First Benennungskonventionen verwenden, können in den meisten Fällen Sie verlassen auf Code First zum Einrichten der Beziehungen zwischen den Tabellen basierend auf den Fremdschlüssel und die Navigationseigenschaften, die Sie in den Klassen definieren.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-107">If you use the Code First naming conventions, in most cases you can rely on Code First to set up relationships between your tables based on the foreign keys and navigation properties that you define on the classes.</span></span> <span data-ttu-id="a7dd3-108">Wenn Sie nicht die Konventionen beim Definieren von Klassen unterliegen, oder wenn Sie verändern möchten die Konventionen arbeiten, können Sie die fluent-API oder datenanmerkungen verwenden, Ihre Klassen zu konfigurieren, damit der Code First die Beziehungen zwischen Tabellen zugeordnet werden können.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-108">If you do not follow the conventions when defining your classes, or if you want to change the way the conventions work, you can use the fluent API or data annotations to configure your classes so Code First can map the relationships between your tables.</span></span>  

## <a name="introduction"></a><span data-ttu-id="a7dd3-109">Einführung</span><span class="sxs-lookup"><span data-stu-id="a7dd3-109">Introduction</span></span>  

<span data-ttu-id="a7dd3-110">Wenn Sie eine Beziehung mit der fluent-API zu konfigurieren, beginnen Sie mit der EntityTypeConfiguration-Instanz, und Sie dann verwenden Sie die HasRequired HasOptional oder HasMany-Methode, um den Typ der Beziehung anzugeben, die diese Entität teilnimmt.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-110">When configuring a relationship with the fluent API, you start with the EntityTypeConfiguration instance and then use the HasRequired, HasOptional, or HasMany method to specify the type of relationship this entity participates in.</span></span> <span data-ttu-id="a7dd3-111">Die HasRequired und HasOptional Methoden nehmen einen Lambda-Ausdruck, der eine Verweis-Navigationseigenschaft darstellt.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-111">The HasRequired and HasOptional methods take a lambda expression that represents a reference navigation property.</span></span> <span data-ttu-id="a7dd3-112">Die HasMany-Methode nimmt es sich um einen Lambda-Ausdruck, der eine auflistungsnavigationseigenschaft darstellt.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-112">The HasMany method takes a lambda expression that represents a collection navigation property.</span></span> <span data-ttu-id="a7dd3-113">Sie können eine inverse Navigationseigenschaft klicken Sie dann mithilfe der Methoden WithRequired WithOptional und WithMany konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-113">You can then configure an inverse navigation property by using the WithRequired, WithOptional, and WithMany methods.</span></span> <span data-ttu-id="a7dd3-114">Diese Methoden verfügen über Überladungen, die akzeptieren keine Argumente und können verwendet werden, um die Kardinalität mit unidirektionalen Navigationen angeben.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-114">These methods have overloads that do not take arguments and can be used to specify cardinality with unidirectional navigations.</span></span>  

<span data-ttu-id="a7dd3-115">Sie können dann Fremdschlüsseleigenschaften konfigurieren, mit die HasForeignKey-Methode.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-115">You can then configure foreign key properties by using the HasForeignKey method.</span></span> <span data-ttu-id="a7dd3-116">Diese Methode verwendet einen Lambda-Ausdruck ein, der die darstellt, die als Fremdschlüssel verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-116">This method takes a lambda expression that represents the property to be used as the foreign key.</span></span>  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a><span data-ttu-id="a7dd3-117">Konfigurieren eine erforderlich, um – optionale Beziehung (One-– 0 (null) oder-1)</span><span class="sxs-lookup"><span data-stu-id="a7dd3-117">Configuring a Required-to-Optional Relationship (One-to–Zero-or-One)</span></span>  

<span data-ttu-id="a7dd3-118">Im folgenden Beispiel wird eine 1: 0 (null)-oder-1-Beziehung.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-118">The following example configures a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="a7dd3-119">Die "OfficeAssignment" verfügt über die InstructorID-Eigenschaft, die einen Primärschlüssel und ein Fremdschlüssel, aus, da der Name der Eigenschaft nicht die Konvention folgt, die die HasKey-Methode verwendet wird, so konfigurieren Sie den primären Schlüssel.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-119">The OfficeAssignment has the InstructorID property that is a primary key and a foreign key, because the name of the property does not follow the convention the HasKey method is used to configure the primary key.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a><span data-ttu-id="a7dd3-120">Konfigurieren eine Beziehung, in dem beide Enden (eins zu eins) erforderlich sind</span><span class="sxs-lookup"><span data-stu-id="a7dd3-120">Configuring a Relationship Where Both Ends Are Required (One-to-One)</span></span>  

<span data-ttu-id="a7dd3-121">In den meisten Fällen kann Entity Framework ableiten, welche Art der abhängigen und der Prinzipal in einer Beziehung.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-121">In most cases Entity Framework can infer which type is the dependent and which is the principal in a relationship.</span></span> <span data-ttu-id="a7dd3-122">Wenn beide Enden der Beziehung erforderlich sind, oder beide Seiten sind optionale Entity Framework können jedoch nicht dem abhängigen und der Prinzipal identifizieren.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-122">However, when both ends of the relationship are required or both sides are optional Entity Framework cannot identify the dependent and principal.</span></span> <span data-ttu-id="a7dd3-123">Wenn beide Enden der Beziehung erforderlich sind, verwenden Sie nach der Methode HasRequired WithRequiredPrincipal oder WithRequiredDependent an.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-123">When both ends of the relationship are required, use WithRequiredPrincipal or WithRequiredDependent after the HasRequired method.</span></span> <span data-ttu-id="a7dd3-124">Wenn beide Enden der Beziehung optional sind, verwenden Sie WithOptionalPrincipal oder WithOptionalDependent, nach der HasOptional-Methode.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-124">When both ends of the relationship are optional, use WithOptionalPrincipal or WithOptionalDependent after the HasOptional method.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a><span data-ttu-id="a7dd3-125">Konfigurieren eine m: N Beziehung</span><span class="sxs-lookup"><span data-stu-id="a7dd3-125">Configuring a Many-to-Many Relationship</span></span>  

<span data-ttu-id="a7dd3-126">Der folgende Code konfiguriert eine m: n Beziehung zwischen den Kurs und "Instructor".</span><span class="sxs-lookup"><span data-stu-id="a7dd3-126">The following code configures a many-to-many relationship between the Course and Instructor types.</span></span> <span data-ttu-id="a7dd3-127">Im folgenden Beispiel werden die Standard-Code First-Konventionen zum Erstellen einer Jointabelle verwendet.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-127">In the following example, the default Code First conventions are used to create a join table.</span></span> <span data-ttu-id="a7dd3-128">Daher wird die Tabelle CourseInstructor mit Course_CourseID und Instructor_InstructorID Spalten erstellt.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-128">As a result the CourseInstructor table is created with Course_CourseID and Instructor_InstructorID columns.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

<span data-ttu-id="a7dd3-129">Wenn Sie den Namen der Jointabelle und die Namen der Spalten in der Tabelle angeben möchten, müssen Sie zusätzliche Konfigurationsschritte ausführen, mit der Map-Methode.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-129">If you want to specify the join table name and the names of the columns in the table you need to do additional configuration by using the Map method.</span></span> <span data-ttu-id="a7dd3-130">Der folgende Code generiert die CourseInstructor-Tabelle mit den Spalten "CourseID" und "InstructorID.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-130">The following code generates the CourseInstructor table with CourseID and InstructorID columns.</span></span>  

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

## <a name="configuring-a-relationship-with-one-navigation-property"></a><span data-ttu-id="a7dd3-131">Konfigurieren eine Beziehung mit einer Navigationseigenschaft</span><span class="sxs-lookup"><span data-stu-id="a7dd3-131">Configuring a Relationship with One Navigation Property</span></span>  

<span data-ttu-id="a7dd3-132">Ein unidirektionales (auch als unidirektional) Beziehung ist, wenn eine Navigationseigenschaft auf nur eine Beziehung enden und nicht auf beide definiert ist.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-132">A one-directional (also called unidirectional) relationship is when a navigation property is defined on only one of the relationship ends and not on both.</span></span> <span data-ttu-id="a7dd3-133">Gemäß der Konvention interpretiert Code First immer als 1: n eine unidirektionale Beziehung.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-133">By convention, Code First always interprets a unidirectional relationship as one-to-many.</span></span> <span data-ttu-id="a7dd3-134">Wenn Sie möchten eine direkte Beziehung zwischen "Instructor" und "OfficeAssignment", wenn Sie eine Navigationseigenschaft für nur den Typ "Instructor" verfügen, müssen Sie z. B. die fluent-API verwenden, um diese Beziehung zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-134">For example, if you want a one-to-one relationship between Instructor and OfficeAssignment, where you have a navigation property on only the Instructor type, you need to use the fluent API to configure this relationship.</span></span>  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a><span data-ttu-id="a7dd3-135">Aktivieren der Löschweitergabe</span><span class="sxs-lookup"><span data-stu-id="a7dd3-135">Enabling Cascade Delete</span></span>  

<span data-ttu-id="a7dd3-136">Sie können die Löschweitergabe auf eine Beziehung konfigurieren, mit der WillCascadeOnDelete-Methode.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-136">You can configure cascade delete on a relationship by using the WillCascadeOnDelete method.</span></span> <span data-ttu-id="a7dd3-137">Ist ein Fremdschlüssel für die abhängige Entität nicht NULL-Werte zulässt, legt dann Code First Löschweitergabe für die Beziehung.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-137">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="a7dd3-138">Wenn ein Fremdschlüssel in der abhängigen Entität NULL-Werte zulässt ist, Code First Löschweitergabe nicht auf der Beziehung fest, und wenn der Prinzipal gelöscht wird die foreign Key wird festgelegt auf Null.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-138">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span>  

<span data-ttu-id="a7dd3-139">Sie können diese Cascade Delete-Konventionen mit entfernen:</span><span class="sxs-lookup"><span data-stu-id="a7dd3-139">You can remove these cascade delete conventions by using:</span></span>  

<span data-ttu-id="a7dd3-140">modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>)</span><span class="sxs-lookup"><span data-stu-id="a7dd3-140">modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>()</span></span>  
<span data-ttu-id="a7dd3-141">modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>)</span><span class="sxs-lookup"><span data-stu-id="a7dd3-141">modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>()</span></span>  

<span data-ttu-id="a7dd3-142">Der folgende Code konfiguriert die Beziehung als erforderlich sein, und anschließend deaktiviert er Löschweitergabe.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-142">The following code configures the relationship to be required and then disables cascade delete.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a><span data-ttu-id="a7dd3-143">Konfigurieren einen zusammengesetzten Foreign Key</span><span class="sxs-lookup"><span data-stu-id="a7dd3-143">Configuring a Composite Foreign Key</span></span>  

<span data-ttu-id="a7dd3-144">Wenn Sie der primäre Schlüssel für den Typ der Abteilung "DepartmentID" und der Namenseigenschaften zusammensetzte, würden Sie den primären Schlüssel für die Abteilung und die foreign Key für die Kurs-Typen wie folgt konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="a7dd3-144">If the primary key on the Department type consisted of DepartmentID and Name properties, you would configure the primary key for the Department and the foreign key on the Course types as follows:</span></span>  

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

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="a7dd3-145">Umbenennen von einem Fremdschlüssel, die nicht im Modell definiert ist</span><span class="sxs-lookup"><span data-stu-id="a7dd3-145">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="a7dd3-146">Wenn Sie keinen Fremdschlüssel für die CLR-Typ definieren, aber welchen Namen verfügen sollte in der Datenbank angeben möchten, führen Sie folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="a7dd3-146">If you choose not to define a foreign key on the CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a><span data-ttu-id="a7dd3-147">Konfigurieren einen Foreign Key-Namen, die nicht die erste Code-Konvention folgt</span><span class="sxs-lookup"><span data-stu-id="a7dd3-147">Configuring a Foreign Key Name That Does Not Follow the Code First Convention</span></span>  

<span data-ttu-id="a7dd3-148">Wenn die Fremdschlüsseleigenschaft für die Kurs-Klasse SomeDepartmentID anstelle von "DepartmentID" aufgerufen wurde, müssten Sie die folgenden Schritte zum angeben, dass Sie SomeDepartmentID der Fremdschlüssel sein soll:</span><span class="sxs-lookup"><span data-stu-id="a7dd3-148">If the foreign key property on the Course class was called SomeDepartmentID instead of DepartmentID, you would need to do the following to specify that you want SomeDepartmentID to be the foreign key:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a><span data-ttu-id="a7dd3-149">In den Beispielen verwendete Modell</span><span class="sxs-lookup"><span data-stu-id="a7dd3-149">Model Used in Samples</span></span>  

<span data-ttu-id="a7dd3-150">Der folgende Code First-Modell wird für die Beispiele auf dieser Seite verwendet.</span><span class="sxs-lookup"><span data-stu-id="a7dd3-150">The following Code First model is used for the samples on this page.</span></span>  

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
