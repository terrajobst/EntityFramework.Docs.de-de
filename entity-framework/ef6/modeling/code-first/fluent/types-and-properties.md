---
title: Fluent-API - Konfiguration und Zuordnen von Eigenschaften und Typen - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
ms.openlocfilehash: e65a3f4721e5c28de63d143e1143f3584e145477
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996986"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a><span data-ttu-id="eb72c-102">Fluent-API – konfigurieren und Zuordnen von Eigenschaften und Typen</span><span class="sxs-lookup"><span data-stu-id="eb72c-102">Fluent API - Configuring and Mapping Properties and Types</span></span>
<span data-ttu-id="eb72c-103">Beim Arbeiten mit Entity Framework Code First ist das Standardverhalten, Tabellen, die mit einem Satz von Konventionen, die in EF integriert die POCO-Klassen zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="eb72c-103">When working with Entity Framework Code First the default behavior is to map your POCO classes to tables using a set of conventions baked into EF.</span></span> <span data-ttu-id="eb72c-104">In einigen Fällen jedoch, Sie können nicht oder möchten, befolgen diese Konventionen und Zuordnen von Entitäten mit etwas anderem als Was vorgeben, dass die Konventionen müssen nicht.</span><span class="sxs-lookup"><span data-stu-id="eb72c-104">Sometimes, however, you cannot or do not want to follow those conventions and need to map entities to something other than what the conventions dictate.</span></span>  

<span data-ttu-id="eb72c-105">Es gibt zwei Hauptmethoden, Sie können konfigurieren, dass EF Verwendung einen anderen Wert als Konventionen, d. h. [Anmerkungen](~/ef6/modeling/code-first/data-annotations.md) oder EFs fluent-API.</span><span class="sxs-lookup"><span data-stu-id="eb72c-105">There are two main ways you can configure EF to use something other than conventions, namely [annotations](~/ef6/modeling/code-first/data-annotations.md) or EFs fluent API.</span></span> <span data-ttu-id="eb72c-106">Die Anmerkungen werden nur eine Teilmenge der fluent-API-Funktionen behandelt, daher gibt es zuordnungsszenarien, die mithilfe von Anmerkungen erreicht werden können.</span><span class="sxs-lookup"><span data-stu-id="eb72c-106">The annotations only cover a subset of the fluent API functionality, so there are mapping scenarios that cannot be achieved using annotations.</span></span> <span data-ttu-id="eb72c-107">In diesem Artikel dient zu zeigen, wie die fluent-API verwenden, um Eigenschaften zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="eb72c-107">This article is designed to demonstrate how to use the fluent API to configure properties.</span></span>  

<span data-ttu-id="eb72c-108">Die erste fluent-API des Codes erfolgt meist durch Überschreiben der ["onmodelcreating"](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) Methode für Ihre abgeleiteten ["DbContext"](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="eb72c-108">The code first fluent API is most commonly accessed by overriding the [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) method on your derived [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span></span> <span data-ttu-id="eb72c-109">Die folgenden Beispiele zeigen, wie verschiedene Aufgaben mit der fluent-api und ermöglichen es Ihnen, kopieren Sie den Code und passen sie Ihr Modell entsprechend an, wenn Sie möchten das Modell, das sie als mit verwendet werden können, finden Sie unter sollen – ist es am Ende dieses Artikels bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="eb72c-109">The following samples are designed to show how to do various tasks with the fluent api and allow you to copy the code out and customize it to suit your model, if you wish to see the model that they can be used with as-is then it is provided at the end of this article.</span></span>  

## <a name="model-wide-settings"></a><span data-ttu-id="eb72c-110">Einstellungen für den gesamten Model</span><span class="sxs-lookup"><span data-stu-id="eb72c-110">Model-Wide Settings</span></span>  

### <a name="default-schema-ef6-onwards"></a><span data-ttu-id="eb72c-111">Standardschema (EF6 oder höher)</span><span class="sxs-lookup"><span data-stu-id="eb72c-111">Default Schema (EF6 onwards)</span></span>  

<span data-ttu-id="eb72c-112">Ab mit EF6 können Sie die "hasdefaultschema"-Methode auf DbModelBuilder verwenden, geben Sie das Datenbankschema für alle Tabellen, gespeicherten Prozeduren usw. verwendet. Für alle Objekte, die Sie explizit ein anderes Schema für konfigurieren, wird diese Standardeinstellung überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="eb72c-112">Starting with EF6 you can use the HasDefaultSchema method on DbModelBuilder to specify the database schema to use for all tables, stored procedures, etc. This default setting will be overridden for any objects that you explicitly configure a different schema for.</span></span>  

``` csharp
modelBuilder.HasDefaultSchema(“sales”);
```  

### <a name="custom-conventions-ef6-onwards"></a><span data-ttu-id="eb72c-113">Benutzerdefinierte Konventionen (EF6 oder höher)</span><span class="sxs-lookup"><span data-stu-id="eb72c-113">Custom Conventions (EF6 onwards)</span></span>  

<span data-ttu-id="eb72c-114">Starten mit EF6 können Sie Ihre eigenen Konventionen zur Ergänzung der enthaltenen in Code First erstellen.</span><span class="sxs-lookup"><span data-stu-id="eb72c-114">Starting with EF6 you can create your own conventions to supplement the ones included in Code First.</span></span> <span data-ttu-id="eb72c-115">Weitere Informationen finden Sie unter [benutzerdefinierte Code First-Konventionen](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="eb72c-115">For more details, see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>  

## <a name="property-mapping"></a><span data-ttu-id="eb72c-116">Eigenschaftszuordnung</span><span class="sxs-lookup"><span data-stu-id="eb72c-116">Property Mapping</span></span>  

<span data-ttu-id="eb72c-117">Die [Eigenschaft](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) Methode dient zum Konfigurieren von Attributen für jede Eigenschaft, die für eine Entität oder einen komplexen Typ gehören.</span><span class="sxs-lookup"><span data-stu-id="eb72c-117">The [Property](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) method is used to configure attributes for each property belonging to an entity or complex type.</span></span> <span data-ttu-id="eb72c-118">Die Property-Methode wird verwendet, um ein Konfigurationsobjekt für eine bestimmte Eigenschaft abzurufen.</span><span class="sxs-lookup"><span data-stu-id="eb72c-118">The Property method is used to obtain a configuration object for a given property.</span></span> <span data-ttu-id="eb72c-119">Die Optionen auf das Konfigurationsobjekt sind spezifisch für den Typ, der konfiguriert wird. IsUnicode sind z. B. nur Zeichenfolgeneigenschaften verfügbar.</span><span class="sxs-lookup"><span data-stu-id="eb72c-119">The options on the configuration object are specific to the type being configured; IsUnicode is available only on string properties for example.</span></span>  

### <a name="configuring-a-primary-key"></a><span data-ttu-id="eb72c-120">Konfigurieren einen Primärschlüssel</span><span class="sxs-lookup"><span data-stu-id="eb72c-120">Configuring a Primary Key</span></span>  

<span data-ttu-id="eb72c-121">Entity Framework wird für Primärschlüssel verwendet:</span><span class="sxs-lookup"><span data-stu-id="eb72c-121">The Entity Framework convention for primary keys is:</span></span>  

1. <span data-ttu-id="eb72c-122">Die Klasse definiert eine Eigenschaft, deren Name "ID" oder "Id" ist.</span><span class="sxs-lookup"><span data-stu-id="eb72c-122">Your class defines a property whose name is “ID” or “Id”</span></span>  
2. <span data-ttu-id="eb72c-123">oder einen Klassennamen gefolgt von "ID" oder "Id"</span><span class="sxs-lookup"><span data-stu-id="eb72c-123">or a class name followed by “ID” or “Id”</span></span>  

<span data-ttu-id="eb72c-124">Um explizit eine Eigenschaft als Primärschlüssel festzulegen, können Sie die HasKey-Methode verwenden.</span><span class="sxs-lookup"><span data-stu-id="eb72c-124">To explicitly set a property to be a primary key, you can use the HasKey method.</span></span> <span data-ttu-id="eb72c-125">Im folgenden Beispiel wird die HasKey-Methode verwendet, konfigurieren Sie den Primärschlüssel InstructorID für die "OfficeAssignment".</span><span class="sxs-lookup"><span data-stu-id="eb72c-125">In the following example, the HasKey method is used to configure the InstructorID primary key on the OfficeAssignment type.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a><span data-ttu-id="eb72c-126">Konfigurieren einen zusammengesetzten Primärschlüssel</span><span class="sxs-lookup"><span data-stu-id="eb72c-126">Configuring a Composite Primary Key</span></span>  

<span data-ttu-id="eb72c-127">Im folgenden Beispiel konfiguriert die Eigenschaften "DepartmentID" und den Namen der zusammengesetzte Primärschlüssel der den Typ der Abteilung werden.</span><span class="sxs-lookup"><span data-stu-id="eb72c-127">The following example configures the DepartmentID and Name properties to be the composite primary key of the Department type.</span></span>  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a><span data-ttu-id="eb72c-128">Identität abschalten für numerische Primärschlüssel</span><span class="sxs-lookup"><span data-stu-id="eb72c-128">Switching off Identity for Numeric Primary Keys</span></span>  

<span data-ttu-id="eb72c-129">Im folgenden Beispiel wird die Eigenschaft "DepartmentID", um System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None, um anzugeben, dass der Wert nicht von der Datenbank generiert wird.</span><span class="sxs-lookup"><span data-stu-id="eb72c-129">The following example sets the DepartmentID property to System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None to indicate that the value will not be generated by the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a><span data-ttu-id="eb72c-130">Die maximale Länge angeben für eine Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="eb72c-130">Specifying the Maximum Length on a Property</span></span>  

<span data-ttu-id="eb72c-131">Im folgenden Beispiel soll die Name-Eigenschaft nicht mehr als 50 Zeichen sein.</span><span class="sxs-lookup"><span data-stu-id="eb72c-131">In the following example, the Name property should be no longer than 50 characters.</span></span> <span data-ttu-id="eb72c-132">Wenn Sie den Wert mit mehr als 50 Zeichen haben, erhalten Sie eine [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) Ausnahme.</span><span class="sxs-lookup"><span data-stu-id="eb72c-132">If you make the value longer than 50 characters, you will get a [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) exception.</span></span> <span data-ttu-id="eb72c-133">Wenn Code First eine Datenbank aus diesem Modell erstellt, wird es auch die maximale Länge der Spalte "Name" auf 50 Zeichen festgelegt.</span><span class="sxs-lookup"><span data-stu-id="eb72c-133">If Code First creates a database from this model it will also set the maximum length of the Name column to 50 characters.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a><span data-ttu-id="eb72c-134">Konfigurieren der Eigenschaft als erforderlich</span><span class="sxs-lookup"><span data-stu-id="eb72c-134">Configuring the Property to be Required</span></span>  

<span data-ttu-id="eb72c-135">Im folgenden Beispiel ist die Name-Eigenschaft erforderlich.</span><span class="sxs-lookup"><span data-stu-id="eb72c-135">In the following example, the Name property is required.</span></span> <span data-ttu-id="eb72c-136">Wenn Sie nicht den Namen angeben, erhalten Sie eine DbEntityValidationException-Ausnahme.</span><span class="sxs-lookup"><span data-stu-id="eb72c-136">If you do not specify the Name, you will get a DbEntityValidationException exception.</span></span> <span data-ttu-id="eb72c-137">Wenn Code First eine Datenbank aus diesem Modell erstellt wird die Spalte verwendet, um diese Eigenschaft gespeichert in der Regel nicht auf NULL festlegbare sein.</span><span class="sxs-lookup"><span data-stu-id="eb72c-137">If Code First creates a database from this model then the column used to store this property will usually be non-nullable.</span></span>  

> [!NOTE]
> <span data-ttu-id="eb72c-138">In einigen Fällen kann es nicht möglich für die Spalte in der Datenbank NULL-Werte zulässt, obwohl die Eigenschaft erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="eb72c-138">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="eb72c-139">Beispielsweise wird bei Verwendung einer TPH-Vererbung Strategie für die Datentyps für mehrere Typen in einer einzelnen Tabelle gespeichert.</span><span class="sxs-lookup"><span data-stu-id="eb72c-139">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="eb72c-140">Wenn ein abgeleiteter Typ eine erforderliche Eigenschaft, die die Spalte NULL-hergestellt werden kann enthält, da nicht alle Typen in der Hierarchie dieser Eigenschaft hat.</span><span class="sxs-lookup"><span data-stu-id="eb72c-140">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a><span data-ttu-id="eb72c-141">Konfigurieren einen Index für eine oder mehrere Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="eb72c-141">Configuring an Index on one or more properties</span></span>  

> [!NOTE]
> <span data-ttu-id="eb72c-142">**EF6.1 oder höher, nur** -der Index-Attribut wurde in Entity Framework 6.1 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="eb72c-142">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="eb72c-143">Die Informationen in diesem Abschnitt gelten nicht, wenn Sie eine frühere Version verwenden.</span><span class="sxs-lookup"><span data-stu-id="eb72c-143">If you are using an earlier version the information in this section does not apply.</span></span>  

<span data-ttu-id="eb72c-144">Erstellen von Indizes wird nicht systemeigen von der Fluent-API unterstützt, Sie können jedoch mithilfe der Unterstützung für **IndexAttribute** über die Fluent-API.</span><span class="sxs-lookup"><span data-stu-id="eb72c-144">Creating indexes isn't natively supported by the Fluent API, but you can make use of the support for **IndexAttribute** via the Fluent API.</span></span> <span data-ttu-id="eb72c-145">Indexattribute werden verarbeitet, dazu eine Modell-Anmerkung auf das Modell, das dann in einen Index in der Datenbank später in der Pipeline aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="eb72c-145">Index attributes are processed by including a model annotation on the model that is then turned into an Index in the database later in the pipeline.</span></span> <span data-ttu-id="eb72c-146">Sie können diese gleiche manuell Anmerkungen, die mithilfe der Fluent-API hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="eb72c-146">You can manually add these same annotations using the Fluent API.</span></span>  

<span data-ttu-id="eb72c-147">Die einfachste Möglichkeit hierzu ist die Erstellung von einer Instanz von **IndexAttribute** , die alle Einstellungen für den neuen Index enthält.</span><span class="sxs-lookup"><span data-stu-id="eb72c-147">The easiest way to do this is to create an instance of **IndexAttribute** that contains all the settings for the new index.</span></span> <span data-ttu-id="eb72c-148">Anschließend können Sie erstellen eine Instanz von **IndexAnnotation** Dies ist eine spezifische EF-Typ, der konvertiert die **IndexAttribute** Einstellungen in einem Modell-Anmerkung, die auf das EF-Modell gespeichert werden können.</span><span class="sxs-lookup"><span data-stu-id="eb72c-148">You can then create an instance of **IndexAnnotation** which is an EF specific type that will convert the **IndexAttribute** settings into a model annotation that can be stored on the EF model.</span></span> <span data-ttu-id="eb72c-149">Diese können anschließend zum Übergeben der **HasColumnAnnotation** Methode für die Fluent-API, Angeben des Namens **Index** für die Anmerkung.</span><span class="sxs-lookup"><span data-stu-id="eb72c-149">These can then be passed to the **HasColumnAnnotation** method on the Fluent API, specifying the name **Index** for the annotation.</span></span>  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

<span data-ttu-id="eb72c-150">Eine vollständige Liste von den Einstellungen in **IndexAttribute**, finden Sie unter den *Index* Abschnitt [Code der ersten Datenanmerkungen](~/ef6/modeling/code-first/data-annotations.md).</span><span class="sxs-lookup"><span data-stu-id="eb72c-150">For a complete list of the settings available in **IndexAttribute**, see the *Index* section of [Code First Data Annotations](~/ef6/modeling/code-first/data-annotations.md).</span></span> <span data-ttu-id="eb72c-151">Dies schließt den Namen des Indexes anpassen, Erstellen eindeutiger Indizes und Indizes mit mehreren Spalten erstellen.</span><span class="sxs-lookup"><span data-stu-id="eb72c-151">This includes customizing the index name, creating unique indexes, and creating multi-column indexes.</span></span>  

<span data-ttu-id="eb72c-152">Sie können mehrere Index Anmerkungen für eine einzelne Eigenschaft angeben, durch Übergeben eines Arrays von **IndexAttribute** an den Konstruktor der **IndexAnnotation**.</span><span class="sxs-lookup"><span data-stu-id="eb72c-152">You can specify multiple index annotations on a single property by passing an array of **IndexAttribute** to the constructor of **IndexAnnotation**.</span></span>  

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

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a><span data-ttu-id="eb72c-153">Angeben nicht um eine CLR-Eigenschaft einer Spalte in der Datenbank zuzuordnen</span><span class="sxs-lookup"><span data-stu-id="eb72c-153">Specifying Not to Map a CLR Property to a Column in the Database</span></span>  

<span data-ttu-id="eb72c-154">Das folgende Beispiel zeigt, wie Sie angeben, dass eine Eigenschaft für einen CLR-Typ nicht auf eine Spalte in der Datenbank zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="eb72c-154">The following example shows how to specify that a property on a CLR type is not mapped to a column in the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a><span data-ttu-id="eb72c-155">Eine CLR-Eigenschaft zuordnen zu einer bestimmten Spalte in der Datenbank</span><span class="sxs-lookup"><span data-stu-id="eb72c-155">Mapping a CLR Property to a Specific Column in the Database</span></span>  

<span data-ttu-id="eb72c-156">Das folgende Beispiel ordnet die Namen von CLR-Eigenschaft in der Datenbankspalte DepartmentName.</span><span class="sxs-lookup"><span data-stu-id="eb72c-156">The following example maps the Name CLR property to the DepartmentName database column.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="eb72c-157">Umbenennen von einem Fremdschlüssel, die nicht im Modell definiert ist</span><span class="sxs-lookup"><span data-stu-id="eb72c-157">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="eb72c-158">Wenn Sie keinen Fremdschlüssel für einen CLR-Typ definieren, aber welchen Namen verfügen sollte in der Datenbank angeben möchten, führen Sie folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="eb72c-158">If you choose not to define a foreign key on a CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a><span data-ttu-id="eb72c-159">Konfigurieren, ob eine Zeichenfolgeneigenschaft Unicode unterstützt.</span><span class="sxs-lookup"><span data-stu-id="eb72c-159">Configuring whether a String Property Supports Unicode Content</span></span>  

<span data-ttu-id="eb72c-160">Standardmäßig werden Zeichenfolgen mit Unicode-(Nvarchar in SQL Server).</span><span class="sxs-lookup"><span data-stu-id="eb72c-160">By default strings are Unicode (nvarchar in SQL Server).</span></span> <span data-ttu-id="eb72c-161">Die IsUnicode-Methode können angeben, dass eine Zeichenfolge des Varchar-Typ werden soll.</span><span class="sxs-lookup"><span data-stu-id="eb72c-161">You can use the IsUnicode method to specify that a string should be of varchar type.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a><span data-ttu-id="eb72c-162">Konfigurieren den Datentyp einer Datenbankspalte</span><span class="sxs-lookup"><span data-stu-id="eb72c-162">Configuring the Data Type of a Database Column</span></span>  

<span data-ttu-id="eb72c-163">Die [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) Methode ermöglicht die Zuordnung auf unterschiedliche Darstellungen des gleichen Typs basic.</span><span class="sxs-lookup"><span data-stu-id="eb72c-163">The [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) method enables mapping to different representations of the same basic type.</span></span> <span data-ttu-id="eb72c-164">Mit dieser Methode kann nicht zum Ausführen der Konvertierung der Daten zur Laufzeit werden.</span><span class="sxs-lookup"><span data-stu-id="eb72c-164">Using this method does not enable you to perform any conversion of the data at run time.</span></span> <span data-ttu-id="eb72c-165">Beachten Sie, dass IsUnicode die bevorzugte Methode der Einstellung Spalten für Varchar, da sie die Datenbank unabhängig ist.</span><span class="sxs-lookup"><span data-stu-id="eb72c-165">Note that IsUnicode is the preferred way of setting columns to varchar, as it is database agnostic.</span></span>  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a><span data-ttu-id="eb72c-166">Konfigurieren von Eigenschaften für einen komplexen Typ</span><span class="sxs-lookup"><span data-stu-id="eb72c-166">Configuring Properties on a Complex Type</span></span>  

<span data-ttu-id="eb72c-167">Es gibt zwei Möglichkeiten, skalare Eigenschaften für einen komplexen Typ zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="eb72c-167">There are two ways to configure scalar properties on a complex type.</span></span>  

<span data-ttu-id="eb72c-168">Sie können die Eigenschaft für ComplexTypeConfiguration aufrufen.</span><span class="sxs-lookup"><span data-stu-id="eb72c-168">You can call Property on ComplexTypeConfiguration.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

<span data-ttu-id="eb72c-169">Sie können auch die punktierte Schreibweise verwenden, Zugriff auf eine Eigenschaft eines komplexen Typs.</span><span class="sxs-lookup"><span data-stu-id="eb72c-169">You can also use the dot notation to access a property of a complex type.</span></span>  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a><span data-ttu-id="eb72c-170">Konfigurieren eine Eigenschaft als Token für vollständige Parallelität verwendet werden soll</span><span class="sxs-lookup"><span data-stu-id="eb72c-170">Configuring a Property to Be Used as an Optimistic Concurrency Token</span></span>  

<span data-ttu-id="eb72c-171">Um anzugeben, dass eine Eigenschaft in einer Entität ein parallelitätstoken darstellt, können Sie das Attribut ConcurrencyCheck oder die IsConcurrencyToken-Methode.</span><span class="sxs-lookup"><span data-stu-id="eb72c-171">To specify that a property in an entity represents a concurrency token, you can use either the ConcurrencyCheck attribute or the IsConcurrencyToken method.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

<span data-ttu-id="eb72c-172">Sie können auch die IsRowVersion-Methode verwenden, so konfigurieren Sie die Eigenschaft als Zeilenversion in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="eb72c-172">You can also use the IsRowVersion method to configure the property to be a row version in the database.</span></span> <span data-ttu-id="eb72c-173">Festlegen der Eigenschaft sein, dass eine Zeilenversion, um ein Token für vollständige Parallelität werden automatisch konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="eb72c-173">Setting the property to be a row version automatically configures it to be an optimistic concurrency token.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a><span data-ttu-id="eb72c-174">Typzuordnung</span><span class="sxs-lookup"><span data-stu-id="eb72c-174">Type Mapping</span></span>  

### <a name="specifying-that-a-class-is-a-complex-type"></a><span data-ttu-id="eb72c-175">Gibt an, dass eine Klasse einen komplexen Typ handelt.</span><span class="sxs-lookup"><span data-stu-id="eb72c-175">Specifying That a Class Is a Complex Type</span></span>  

<span data-ttu-id="eb72c-176">Gemäß der Konvention ist ein Typ, der keinen primären Schlüssel angegeben hat, als komplexer Typ behandelt.</span><span class="sxs-lookup"><span data-stu-id="eb72c-176">By convention, a type that has no primary key specified is treated as a complex type.</span></span> <span data-ttu-id="eb72c-177">Es gibt einige Szenarien, in denen Code First keinen komplexen Typ erkannt wird (z. B., wenn Sie müssen eine Eigenschaft namens-ID, aber Sie bedeutet nicht, dass es kein primärer Schlüssel sein).</span><span class="sxs-lookup"><span data-stu-id="eb72c-177">There are some scenarios where Code First will not detect a complex type (for example, if you do have a property called ID, but you do not mean for it to be a primary key).</span></span> <span data-ttu-id="eb72c-178">In solchen Fällen verwenden Sie die fluent-API explizit angeben, dass ein Typ eines komplexen Typs ist.</span><span class="sxs-lookup"><span data-stu-id="eb72c-178">In such cases, you would use the fluent API to explicitly specify that a type is a complex type.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a><span data-ttu-id="eb72c-179">Angeben nicht um eine CLR-Entitätstyp in eine Tabelle in der Datenbank zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="eb72c-179">Specifying Not to Map a CLR Entity Type to a Table in the Database</span></span>  

<span data-ttu-id="eb72c-180">Das folgende Beispiel zeigt, wie Sie ausschließen, dass einen CLR-Typ in eine Tabelle in der Datenbank zugeordnet wird.</span><span class="sxs-lookup"><span data-stu-id="eb72c-180">The following example shows how to exclude a CLR type from being mapped to a table in the database.</span></span>  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a><span data-ttu-id="eb72c-181">Mapping eines Entitätstyps zu einer bestimmten Tabelle in der Datenbank</span><span class="sxs-lookup"><span data-stu-id="eb72c-181">Mapping an Entity Type to a Specific Table in the Database</span></span>  

<span data-ttu-id="eb72c-182">Alle Eigenschaften der Abteilung werden auf Spalten in einer Tabelle mit T_ Abteilung zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="eb72c-182">All properties of Department will be mapped to columns in a table called t_ Department.</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

<span data-ttu-id="eb72c-183">Sie können auch den Schemanamen, wie folgt angeben:</span><span class="sxs-lookup"><span data-stu-id="eb72c-183">You can also specify the schema name like this:</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a><span data-ttu-id="eb72c-184">Die Tabelle pro Hierarchie (TPH)-Vererbung zuordnen</span><span class="sxs-lookup"><span data-stu-id="eb72c-184">Mapping the Table-Per-Hierarchy (TPH) Inheritance</span></span>  

<span data-ttu-id="eb72c-185">In diesem Szenario TPH-Zuordnung werden alle Typen in einer Vererbungshierarchie einer einzelnen Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="eb72c-185">In the TPH mapping scenario, all types in an inheritance hierarchy are mapped to a single table.</span></span> <span data-ttu-id="eb72c-186">Eine Diskriminatorspalte wird verwendet, um den Typ der einzelnen Zeilen zu identifizieren.</span><span class="sxs-lookup"><span data-stu-id="eb72c-186">A discriminator column is used to identify the type of each row.</span></span> <span data-ttu-id="eb72c-187">Wenn Sie Ihr Modell mit Code First zu erstellen, ist der TPH die Standardstrategie für die Typen, die in der Vererbungshierarchie beteiligt sind.</span><span class="sxs-lookup"><span data-stu-id="eb72c-187">When creating your model with Code First, TPH is the default strategy for the types that participate in the inheritance hierarchy.</span></span> <span data-ttu-id="eb72c-188">In der Standardeinstellung die Diskriminatorspalte wird hinzugefügt, auf die Tabelle mit dem Namen "Discriminator genannt", und die CLR-Typnamen jedes Typs in der Hierarchie Diskriminatorwerte verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="eb72c-188">By default, the discriminator column is added to the table with the name “Discriminator” and the CLR type name of each type in the hierarchy is used for the discriminator values.</span></span> <span data-ttu-id="eb72c-189">Sie können das Standardverhalten ändern, mit der fluent-API.</span><span class="sxs-lookup"><span data-stu-id="eb72c-189">You can modify the default behavior by using the fluent API.</span></span>  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a><span data-ttu-id="eb72c-190">Die Tabelle pro Typ (TPT)-Vererbung zuordnen</span><span class="sxs-lookup"><span data-stu-id="eb72c-190">Mapping the Table-Per-Type (TPT) Inheritance</span></span>  

<span data-ttu-id="eb72c-191">In diesem Szenario TPT-Zuordnung werden alle Typen einzelnen Tabellen zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="eb72c-191">In the TPT mapping scenario, all types are mapped to individual tables.</span></span> <span data-ttu-id="eb72c-192">Eigenschaften, die nur zu einem Basistyp oder einem abgeleiteten Typ gehören, werden in einer Tabelle gespeichert, die diesem Typ zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="eb72c-192">Properties that belong solely to a base type or derived type are stored in a table that maps to that type.</span></span> <span data-ttu-id="eb72c-193">Tabellen, die für abgeleitete Typen zuordnen Speichern auch Fremdschlüssel, der die abgeleitete Tabelle mit der Basistabelle verknüpft.</span><span class="sxs-lookup"><span data-stu-id="eb72c-193">Tables that map to derived types also store a foreign key that joins the derived table with the base table.</span></span>  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a><span data-ttu-id="eb72c-194">Die Tabelle pro konkreter Klasse (TPC)-Vererbung zuordnen</span><span class="sxs-lookup"><span data-stu-id="eb72c-194">Mapping the Table-Per-Concrete Class (TPC) Inheritance</span></span>  

<span data-ttu-id="eb72c-195">In diesem Szenario TPC-Zuordnung werden einzelne Tabellen alle nicht abstrakten Typen in der Hierarchie zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="eb72c-195">In the TPC mapping scenario, all non-abstract types in the hierarchy are mapped to individual tables.</span></span> <span data-ttu-id="eb72c-196">Die Tabellen, die die abgeleiteten Klassen zugeordnet, haben keine Beziehung zur Tabelle, die die Basisklasse in der Datenbank zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="eb72c-196">The tables that map to the derived classes have no relationship to the table that maps to the base class in the database.</span></span> <span data-ttu-id="eb72c-197">Alle Eigenschaften einer Klasse, einschließlich der geerbten Eigenschaften sind Spalten der entsprechenden Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="eb72c-197">All properties of a class, including inherited properties, are mapped to columns of the corresponding table.</span></span>  

<span data-ttu-id="eb72c-198">Rufen Sie die MapInheritedProperties-Methode, um jedem abgeleiteten Typ zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="eb72c-198">Call the MapInheritedProperties method to configure each derived type.</span></span> <span data-ttu-id="eb72c-199">MapInheritedProperties zugeordnet, alle Eigenschaften, die zu neuen Spalten in der Tabelle für die abgeleitete Klasse von der Basisklasse geerbt wurden.</span><span class="sxs-lookup"><span data-stu-id="eb72c-199">MapInheritedProperties remaps all properties that were inherited from the base class to new columns in the table for the derived class.</span></span>  

> [!NOTE]
> <span data-ttu-id="eb72c-200">Beachten Sie, dass da TPC-Vererbungshierarchie an keinen aufweisen gibt es ein Primärschlüssel doppelte Entitätsschlüssel beim Einfügen von in Tabellen, die Unterklassen zugeordnet sind, wenn Sie die generiert Datenbankwerte, mit der gleichen ID-Ausgangswert verfügen.</span><span class="sxs-lookup"><span data-stu-id="eb72c-200">Note that because the tables participating in TPC inheritance hierarchy do not share a primary key there will be duplicate entity keys when inserting in tables that are mapped to subclasses if you have database generated values with the same identity seed.</span></span> <span data-ttu-id="eb72c-201">Zur Lösung dieses Problems können Sie einen anderen Ausgangswert für die einzelnen Tabellen festlegen oder Identität auf die Primärschlüsseleigenschaft abschalten.</span><span class="sxs-lookup"><span data-stu-id="eb72c-201">To solve this problem you can either specify a different initial seed value for each table or switch off identity on the primary key property.</span></span> <span data-ttu-id="eb72c-202">Identität ist der Standardwert für Schlüsseleigenschaften ganze Zahl, bei der Arbeit mit Code First.</span><span class="sxs-lookup"><span data-stu-id="eb72c-202">Identity is the default value for integer key properties when working with Code First.</span></span>  

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

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a><span data-ttu-id="eb72c-203">Zuordnen von Eigenschaften eines Entitätstyps zu mehreren Tabellen in der Datenbank (Entitätsaufteilung)</span><span class="sxs-lookup"><span data-stu-id="eb72c-203">Mapping Properties of an Entity Type to Multiple Tables in the Database (Entity Splitting)</span></span>  

<span data-ttu-id="eb72c-204">Entitätsaufteilung kann die Eigenschaften eines Entitätstyps über mehrere Tabellen verteilt werden.</span><span class="sxs-lookup"><span data-stu-id="eb72c-204">Entity splitting allows the properties of an entity type to be spread across multiple tables.</span></span> <span data-ttu-id="eb72c-205">Im folgenden Beispiel wird die Entität "Department" in zwei Tabellen unterteilt: Abteilung und DepartmentDetails.</span><span class="sxs-lookup"><span data-stu-id="eb72c-205">In the following example, the Department entity is split into two tables: Department and DepartmentDetails.</span></span> <span data-ttu-id="eb72c-206">Die entitätsaufteilung verwendet mehrere Aufrufe an die Map-Methode, um eine Teilmenge der Eigenschaften einer bestimmten Tabelle zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="eb72c-206">Entity splitting uses multiple calls to the Map method to map a subset of properties to a specific table.</span></span>  

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

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a><span data-ttu-id="eb72c-207">Zuordnen von mehreren Entitätstypen zur Verfügung zu einer Tabelle in der Datenbank (Tabelle teilen)</span><span class="sxs-lookup"><span data-stu-id="eb72c-207">Mapping Multiple Entity Types to One Table in the Database (Table Splitting)</span></span>  

<span data-ttu-id="eb72c-208">Das folgende Beispiel ordnet zwei Entitätstypen, die einen Primärschlüssel für eine Tabelle gemeinsam nutzen.</span><span class="sxs-lookup"><span data-stu-id="eb72c-208">The following example maps two entity types that share a primary key to one table.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a><span data-ttu-id="eb72c-209">Mapping eines Entitätstyps zu gespeicherten Insert/Update/Delete-Prozeduren (EF6 oder höher)</span><span class="sxs-lookup"><span data-stu-id="eb72c-209">Mapping an Entity Type to Insert/Update/Delete Stored Procedures (EF6 onwards)</span></span>  

<span data-ttu-id="eb72c-210">Starten Sie mit EF6 kann eine Entität zum Verwenden von gespeicherten Prozeduren für Insert, Update, und Löschen von zuordnen.</span><span class="sxs-lookup"><span data-stu-id="eb72c-210">Starting with EF6 you can map an entity to use stored procedures for insert update and delete.</span></span> <span data-ttu-id="eb72c-211">Weitere Informationen finden Sie unter [Code erste Insert/Update/Delete gespeicherte Prozeduren](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span><span class="sxs-lookup"><span data-stu-id="eb72c-211">For more details see, [Code First Insert/Update/Delete Stored Procedures](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span></span>  

## <a name="model-used-in-samples"></a><span data-ttu-id="eb72c-212">In den Beispielen verwendete Modell</span><span class="sxs-lookup"><span data-stu-id="eb72c-212">Model Used in Samples</span></span>  

<span data-ttu-id="eb72c-213">Der folgende Code First-Modell wird für die Beispiele auf dieser Seite verwendet.</span><span class="sxs-lookup"><span data-stu-id="eb72c-213">The following Code First model is used for the samples on this page.</span></span>  

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
