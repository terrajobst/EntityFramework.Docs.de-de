---
title: Fluent-API - Konfiguration und Zuordnen von Eigenschaften und Typen - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
ms.openlocfilehash: 7371cc99142ccf8fc6bea237d7d58d1e67fcecec
ms.sourcegitcommit: 75f8a179ac9a70ad390fc7ab2a6c5e714e701b8b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/27/2018
ms.locfileid: "52339802"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a>Fluent-API – konfigurieren und Zuordnen von Eigenschaften und Typen
Beim Arbeiten mit Entity Framework Code First ist das Standardverhalten, Tabellen, die mit einem Satz von Konventionen, die in EF integriert die POCO-Klassen zugeordnet. In einigen Fällen jedoch, Sie können nicht oder möchten, befolgen diese Konventionen und Zuordnen von Entitäten mit etwas anderem als Was vorgeben, dass die Konventionen müssen nicht.  

Es gibt zwei Hauptmethoden, Sie können konfigurieren, dass EF Verwendung einen anderen Wert als Konventionen, d. h. [Anmerkungen](~/ef6/modeling/code-first/data-annotations.md) oder EFs fluent-API. Die Anmerkungen werden nur eine Teilmenge der fluent-API-Funktionen behandelt, daher gibt es zuordnungsszenarien, die mithilfe von Anmerkungen erreicht werden können. In diesem Artikel dient zu zeigen, wie die fluent-API verwenden, um Eigenschaften zu konfigurieren.  

Die erste fluent-API des Codes erfolgt meist durch Überschreiben der ["onmodelcreating"](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) Methode für Ihre abgeleiteten ["DbContext"](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx). Die folgenden Beispiele zeigen, wie verschiedene Aufgaben mit der fluent-api und ermöglichen es Ihnen, kopieren Sie den Code und passen sie Ihr Modell entsprechend an, wenn Sie möchten das Modell, das sie als mit verwendet werden können, finden Sie unter sollen – ist es am Ende dieses Artikels bereitgestellt wird.  

## <a name="model-wide-settings"></a>Einstellungen für den gesamten Model  

### <a name="default-schema-ef6-onwards"></a>Standardschema (EF6 oder höher)  

Ab mit EF6 können Sie die "hasdefaultschema"-Methode auf DbModelBuilder verwenden, geben Sie das Datenbankschema für alle Tabellen, gespeicherten Prozeduren usw. verwendet. Für alle Objekte, die Sie explizit ein anderes Schema für konfigurieren, wird diese Standardeinstellung überschrieben werden.  

``` csharp
modelBuilder.HasDefaultSchema("sales");
```  

### <a name="custom-conventions-ef6-onwards"></a>Benutzerdefinierte Konventionen (EF6 oder höher)  

Starten mit EF6 können Sie Ihre eigenen Konventionen zur Ergänzung der enthaltenen in Code First erstellen. Weitere Informationen finden Sie unter [benutzerdefinierte Code First-Konventionen](~/ef6/modeling/code-first/conventions/custom.md).  

## <a name="property-mapping"></a>Eigenschaftszuordnung  

Die [Eigenschaft](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) Methode dient zum Konfigurieren von Attributen für jede Eigenschaft, die für eine Entität oder einen komplexen Typ gehören. Die Property-Methode wird verwendet, um ein Konfigurationsobjekt für eine bestimmte Eigenschaft abzurufen. Die Optionen auf das Konfigurationsobjekt sind spezifisch für den Typ, der konfiguriert wird. IsUnicode sind z. B. nur Zeichenfolgeneigenschaften verfügbar.  

### <a name="configuring-a-primary-key"></a>Konfigurieren einen Primärschlüssel  

Entity Framework wird für Primärschlüssel verwendet:  

1. Die Klasse definiert eine Eigenschaft, deren Name "ID" oder "Id" ist.  
2. oder einen Klassennamen gefolgt von "ID" oder "Id"  

Um explizit eine Eigenschaft als Primärschlüssel festzulegen, können Sie die HasKey-Methode verwenden. Im folgenden Beispiel wird die HasKey-Methode verwendet, konfigurieren Sie den Primärschlüssel InstructorID für die "OfficeAssignment".  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a>Konfigurieren einen zusammengesetzten Primärschlüssel  

Im folgenden Beispiel konfiguriert die Eigenschaften "DepartmentID" und den Namen der zusammengesetzte Primärschlüssel der den Typ der Abteilung werden.  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a>Identität abschalten für numerische Primärschlüssel  

Im folgenden Beispiel wird die Eigenschaft "DepartmentID", um System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None, um anzugeben, dass der Wert nicht von der Datenbank generiert wird.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a>Die maximale Länge angeben für eine Eigenschaft.  

Im folgenden Beispiel soll die Name-Eigenschaft nicht mehr als 50 Zeichen sein. Wenn Sie den Wert mit mehr als 50 Zeichen haben, erhalten Sie eine [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) Ausnahme. Wenn Code First eine Datenbank aus diesem Modell erstellt, wird es auch die maximale Länge der Spalte "Name" auf 50 Zeichen festgelegt.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a>Konfigurieren der Eigenschaft als erforderlich  

Im folgenden Beispiel ist die Name-Eigenschaft erforderlich. Wenn Sie nicht den Namen angeben, erhalten Sie eine DbEntityValidationException-Ausnahme. Wenn Code First eine Datenbank aus diesem Modell erstellt wird die Spalte verwendet, um diese Eigenschaft gespeichert in der Regel nicht auf NULL festlegbare sein.  

> [!NOTE]
> In einigen Fällen kann es nicht möglich für die Spalte in der Datenbank NULL-Werte zulässt, obwohl die Eigenschaft erforderlich ist. Beispielsweise wird bei Verwendung einer TPH-Vererbung Strategie für die Datentyps für mehrere Typen in einer einzelnen Tabelle gespeichert. Wenn ein abgeleiteter Typ eine erforderliche Eigenschaft, die die Spalte NULL-hergestellt werden kann enthält, da nicht alle Typen in der Hierarchie dieser Eigenschaft hat.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a>Konfigurieren einen Index für eine oder mehrere Eigenschaften  

> [!NOTE]
> **EF6.1 oder höher, nur** -der Index-Attribut wurde in Entity Framework 6.1 eingeführt. Die Informationen in diesem Abschnitt gelten nicht, wenn Sie eine frühere Version verwenden.  

Erstellen von Indizes wird nicht systemeigen von der Fluent-API unterstützt, Sie können jedoch mithilfe der Unterstützung für **IndexAttribute** über die Fluent-API. Indexattribute werden verarbeitet, dazu eine Modell-Anmerkung auf das Modell, das dann in einen Index in der Datenbank später in der Pipeline aktiviert ist. Sie können diese gleiche manuell Anmerkungen, die mithilfe der Fluent-API hinzufügen.  

Die einfachste Möglichkeit hierzu ist die Erstellung von einer Instanz von **IndexAttribute** , die alle Einstellungen für den neuen Index enthält. Anschließend können Sie erstellen eine Instanz von **IndexAnnotation** Dies ist eine spezifische EF-Typ, der konvertiert die **IndexAttribute** Einstellungen in einem Modell-Anmerkung, die auf das EF-Modell gespeichert werden können. Diese können anschließend zum Übergeben der **HasColumnAnnotation** Methode für die Fluent-API, Angeben des Namens **Index** für die Anmerkung.  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

Eine vollständige Liste von den Einstellungen in **IndexAttribute**, finden Sie unter den *Index* Abschnitt [Code der ersten Datenanmerkungen](~/ef6/modeling/code-first/data-annotations.md). Dies schließt den Namen des Indexes anpassen, Erstellen eindeutiger Indizes und Indizes mit mehreren Spalten erstellen.  

Sie können mehrere Index Anmerkungen für eine einzelne Eigenschaft angeben, durch Übergeben eines Arrays von **IndexAttribute** an den Konstruktor der **IndexAnnotation**.  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation(
        "Index",  
        new IndexAnnotation(new[]
            {
                new IndexAttribute("Index1"),
                new IndexAttribute("Index2") { IsUnique = true }
            })));
```  

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a>Angeben nicht um eine CLR-Eigenschaft einer Spalte in der Datenbank zuzuordnen  

Das folgende Beispiel zeigt, wie Sie angeben, dass eine Eigenschaft für einen CLR-Typ nicht auf eine Spalte in der Datenbank zugeordnet ist.  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a>Eine CLR-Eigenschaft zuordnen zu einer bestimmten Spalte in der Datenbank  

Das folgende Beispiel ordnet die Namen von CLR-Eigenschaft in der Datenbankspalte DepartmentName.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Umbenennen von einem Fremdschlüssel, die nicht im Modell definiert ist  

Wenn Sie keinen Fremdschlüssel für einen CLR-Typ definieren, aber welchen Namen verfügen sollte in der Datenbank angeben möchten, führen Sie folgende Schritte aus:  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a>Konfigurieren, ob eine Zeichenfolgeneigenschaft Unicode unterstützt.  

Standardmäßig werden Zeichenfolgen mit Unicode-(Nvarchar in SQL Server). Die IsUnicode-Methode können angeben, dass eine Zeichenfolge des Varchar-Typ werden soll.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a>Konfigurieren den Datentyp einer Datenbankspalte  

Die [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) Methode ermöglicht die Zuordnung auf unterschiedliche Darstellungen des gleichen Typs basic. Mit dieser Methode kann nicht zum Ausführen der Konvertierung der Daten zur Laufzeit werden. Beachten Sie, dass IsUnicode die bevorzugte Methode der Einstellung Spalten für Varchar, da sie die Datenbank unabhängig ist.  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a>Konfigurieren von Eigenschaften für einen komplexen Typ  

Es gibt zwei Möglichkeiten, skalare Eigenschaften für einen komplexen Typ zu konfigurieren.  

Sie können die Eigenschaft für ComplexTypeConfiguration aufrufen.  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

Sie können auch die punktierte Schreibweise verwenden, Zugriff auf eine Eigenschaft eines komplexen Typs.  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a>Konfigurieren eine Eigenschaft als Token für vollständige Parallelität verwendet werden soll  

Um anzugeben, dass eine Eigenschaft in einer Entität ein parallelitätstoken darstellt, können Sie das Attribut ConcurrencyCheck oder die IsConcurrencyToken-Methode.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

Sie können auch die IsRowVersion-Methode verwenden, so konfigurieren Sie die Eigenschaft als Zeilenversion in der Datenbank. Festlegen der Eigenschaft sein, dass eine Zeilenversion, um ein Token für vollständige Parallelität werden automatisch konfiguriert.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a>Typzuordnung  

### <a name="specifying-that-a-class-is-a-complex-type"></a>Gibt an, dass eine Klasse einen komplexen Typ handelt.  

Gemäß der Konvention ist ein Typ, der keinen primären Schlüssel angegeben hat, als komplexer Typ behandelt. Es gibt einige Szenarien, in denen Code First keinen komplexen Typ erkannt wird (z. B., wenn Sie müssen eine Eigenschaft namens-ID, aber Sie bedeutet nicht, dass es kein primärer Schlüssel sein). In solchen Fällen verwenden Sie die fluent-API explizit angeben, dass ein Typ eines komplexen Typs ist.  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a>Angeben nicht um eine CLR-Entitätstyp in eine Tabelle in der Datenbank zugeordnet wird.  

Das folgende Beispiel zeigt, wie Sie ausschließen, dass einen CLR-Typ in eine Tabelle in der Datenbank zugeordnet wird.  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a>Mapping eines Entitätstyps zu einer bestimmten Tabelle in der Datenbank  

Alle Eigenschaften der Abteilung werden auf Spalten in einer Tabelle mit T_ Abteilung zugeordnet werden.  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

Sie können auch den Schemanamen, wie folgt angeben:  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a>Die Tabelle pro Hierarchie (TPH)-Vererbung zuordnen  

In diesem Szenario TPH-Zuordnung werden alle Typen in einer Vererbungshierarchie einer einzelnen Tabelle zugeordnet. Eine Diskriminatorspalte wird verwendet, um den Typ der einzelnen Zeilen zu identifizieren. Wenn Sie Ihr Modell mit Code First zu erstellen, ist der TPH die Standardstrategie für die Typen, die in der Vererbungshierarchie beteiligt sind. In der Standardeinstellung die Diskriminatorspalte wird hinzugefügt, auf die Tabelle mit dem Namen "Discriminator genannt", und die CLR-Typnamen jedes Typs in der Hierarchie Diskriminatorwerte verwendet wird. Sie können das Standardverhalten ändern, mit der fluent-API.  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a>Die Tabelle pro Typ (TPT)-Vererbung zuordnen  

In diesem Szenario TPT-Zuordnung werden alle Typen einzelnen Tabellen zugeordnet. Eigenschaften, die nur zu einem Basistyp oder einem abgeleiteten Typ gehören, werden in einer Tabelle gespeichert, die diesem Typ zugeordnet ist. Tabellen, die für abgeleitete Typen zuordnen Speichern auch Fremdschlüssel, der die abgeleitete Tabelle mit der Basistabelle verknüpft.  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a>Die Tabelle pro konkreter Klasse (TPC)-Vererbung zuordnen  

In diesem Szenario TPC-Zuordnung werden einzelne Tabellen alle nicht abstrakten Typen in der Hierarchie zugeordnet. Die Tabellen, die die abgeleiteten Klassen zugeordnet, haben keine Beziehung zur Tabelle, die die Basisklasse in der Datenbank zugeordnet ist. Alle Eigenschaften einer Klasse, einschließlich der geerbten Eigenschaften sind Spalten der entsprechenden Tabelle zugeordnet.  

Rufen Sie die MapInheritedProperties-Methode, um jedem abgeleiteten Typ zu konfigurieren. MapInheritedProperties zugeordnet, alle Eigenschaften, die zu neuen Spalten in der Tabelle für die abgeleitete Klasse von der Basisklasse geerbt wurden.  

> [!NOTE]
> Beachten Sie, dass da TPC-Vererbungshierarchie an keinen aufweisen gibt es ein Primärschlüssel doppelte Entitätsschlüssel beim Einfügen von in Tabellen, die Unterklassen zugeordnet sind, wenn Sie die generiert Datenbankwerte, mit der gleichen ID-Ausgangswert verfügen. Zur Lösung dieses Problems können Sie einen anderen Ausgangswert für die einzelnen Tabellen festlegen oder Identität auf die Primärschlüsseleigenschaft abschalten. Identität ist der Standardwert für Schlüsseleigenschaften ganze Zahl, bei der Arbeit mit Code First.  

``` csharp
modelBuilder.Entity<Course>()
    .Property(c => c.CourseID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);

modelBuilder.Entity<OnsiteCourse>().Map(m =>
{
    m.MapInheritedProperties();
    m.ToTable("OnsiteCourse");
});

modelBuilder.Entity<OnlineCourse>().Map(m =>
{
    m.MapInheritedProperties();
    m.ToTable("OnlineCourse");
});
```  

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a>Zuordnen von Eigenschaften eines Entitätstyps zu mehreren Tabellen in der Datenbank (Entitätsaufteilung)  

Entitätsaufteilung kann die Eigenschaften eines Entitätstyps über mehrere Tabellen verteilt werden. Im folgenden Beispiel wird die Entität "Department" in zwei Tabellen unterteilt: Abteilung und DepartmentDetails. Die entitätsaufteilung verwendet mehrere Aufrufe an die Map-Methode, um eine Teilmenge der Eigenschaften einer bestimmten Tabelle zuzuordnen.  

``` csharp
modelBuilder.Entity<Department>()
    .Map(m =>
    {
        m.Properties(t => new { t.DepartmentID, t.Name });
        m.ToTable("Department");
    })
    .Map(m =>
    {
        m.Properties(t => new { t.DepartmentID, t.Administrator, t.StartDate, t.Budget });
        m.ToTable("DepartmentDetails");
    });
```  

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a>Zuordnen von mehreren Entitätstypen zur Verfügung zu einer Tabelle in der Datenbank (Tabelle teilen)  

Das folgende Beispiel ordnet zwei Entitätstypen, die einen Primärschlüssel für eine Tabelle gemeinsam nutzen.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a>Mapping eines Entitätstyps zu gespeicherten Insert/Update/Delete-Prozeduren (EF6 oder höher)  

Starten Sie mit EF6 kann eine Entität zum Verwenden von gespeicherten Prozeduren für Insert, Update, und Löschen von zuordnen. Weitere Informationen finden Sie unter [Code erste Insert/Update/Delete gespeicherte Prozeduren](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).  

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
