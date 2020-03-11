---
title: Fließende Eigenschaften und Typen für die API-Konfiguration und-Zuordnung EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
ms.openlocfilehash: 7371cc99142ccf8fc6bea237d7d58d1e67fcecec
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415754"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a><span data-ttu-id="2f0f8-102">Fließende Eigenschaften und Typen für die API-Konfiguration und-Zuordnung</span><span class="sxs-lookup"><span data-stu-id="2f0f8-102">Fluent API - Configuring and Mapping Properties and Types</span></span>
<span data-ttu-id="2f0f8-103">Beim Arbeiten mit Entity Framework Code First das Standardverhalten das Zuordnen Ihrer poco-Klassen zu Tabellen mithilfe eines Satzes von Konventionen, die in EF gebacken werden.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-103">When working with Entity Framework Code First the default behavior is to map your POCO classes to tables using a set of conventions baked into EF.</span></span> <span data-ttu-id="2f0f8-104">Manchmal ist es jedoch nicht möglich, diese Konventionen einzuhalten, und die Entitäten müssen einem anderen als dem von den Konventionen vorgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-104">Sometimes, however, you cannot or do not want to follow those conventions and need to map entities to something other than what the conventions dictate.</span></span>  

<span data-ttu-id="2f0f8-105">Es gibt zwei Hauptmethoden zum Konfigurieren von EF, um etwas anderes als Konventionen zu verwenden, nämlich [Anmerkungen](~/ef6/modeling/code-first/data-annotations.md) oder eine EFS-fließende API.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-105">There are two main ways you can configure EF to use something other than conventions, namely [annotations](~/ef6/modeling/code-first/data-annotations.md) or EFs fluent API.</span></span> <span data-ttu-id="2f0f8-106">Die Anmerkungen decken nur eine Teilmenge der Funktionen der fließenden API ab, daher gibt es zuordnungsszenarien, die mithilfe von Anmerkungen nicht erreicht werden können.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-106">The annotations only cover a subset of the fluent API functionality, so there are mapping scenarios that cannot be achieved using annotations.</span></span> <span data-ttu-id="2f0f8-107">In diesem Artikel wird veranschaulicht, wie Sie die fließende API zum Konfigurieren von Eigenschaften verwenden.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-107">This article is designed to demonstrate how to use the fluent API to configure properties.</span></span>  

<span data-ttu-id="2f0f8-108">Der Zugriff auf die Code First-fließend-API wird am häufigsten durch Überschreiben der [onmodelcreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) -Methode für den abgeleiteten [dbcontext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx)erreicht.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-108">The code first fluent API is most commonly accessed by overriding the [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) method on your derived [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span></span> <span data-ttu-id="2f0f8-109">Die folgenden Beispiele veranschaulichen, wie verschiedene Aufgaben mit der fließend-API ausgeführt werden können und wie Sie den Code kopieren und an Ihr Modell anpassen können. Wenn Sie sehen möchten, wie das Modell, mit dem Sie arbeiten können, unverändert verwendet werden kann, wird es am Ende dieses Artikels bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-109">The following samples are designed to show how to do various tasks with the fluent api and allow you to copy the code out and customize it to suit your model, if you wish to see the model that they can be used with as-is then it is provided at the end of this article.</span></span>  

## <a name="model-wide-settings"></a><span data-ttu-id="2f0f8-110">Modell weite Einstellungen</span><span class="sxs-lookup"><span data-stu-id="2f0f8-110">Model-Wide Settings</span></span>  

### <a name="default-schema-ef6-onwards"></a><span data-ttu-id="2f0f8-111">Standard Schema (EF6 oder höher)</span><span class="sxs-lookup"><span data-stu-id="2f0f8-111">Default Schema (EF6 onwards)</span></span>  

<span data-ttu-id="2f0f8-112">Beginnend mit EF6 können Sie mit der hasdefaultschema-Methode von dbmodelbuilder das Datenbankschema angeben, das für alle Tabellen, gespeicherten Prozeduren usw. verwendet werden soll. Diese Standardeinstellung wird für alle Objekte, für die Sie explizit ein anderes Schema konfigurieren, überschrieben.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-112">Starting with EF6 you can use the HasDefaultSchema method on DbModelBuilder to specify the database schema to use for all tables, stored procedures, etc. This default setting will be overridden for any objects that you explicitly configure a different schema for.</span></span>  

``` csharp
modelBuilder.HasDefaultSchema("sales");
```  

### <a name="custom-conventions-ef6-onwards"></a><span data-ttu-id="2f0f8-113">Benutzerdefinierte Konventionen (EF6 oder höher)</span><span class="sxs-lookup"><span data-stu-id="2f0f8-113">Custom Conventions (EF6 onwards)</span></span>  

<span data-ttu-id="2f0f8-114">Beginnend mit EF6 können Sie eigene Konventionen erstellen, um die in Code First enthaltenen Konventionen zu ergänzen.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-114">Starting with EF6 you can create your own conventions to supplement the ones included in Code First.</span></span> <span data-ttu-id="2f0f8-115">Weitere Informationen finden Sie unter [benutzerdefinierte Code First Konventionen](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="2f0f8-115">For more details, see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>  

## <a name="property-mapping"></a><span data-ttu-id="2f0f8-116">Eigenschaftszuordnung</span><span class="sxs-lookup"><span data-stu-id="2f0f8-116">Property Mapping</span></span>  

<span data-ttu-id="2f0f8-117">Die [Property](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) -Methode wird verwendet, um Attribute für jede Eigenschaft zu konfigurieren, die zu einer Entität oder einem komplexen Typ gehört.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-117">The [Property](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) method is used to configure attributes for each property belonging to an entity or complex type.</span></span> <span data-ttu-id="2f0f8-118">Die-Eigenschaften Methode wird verwendet, um ein Konfigurationsobjekt für eine bestimmte Eigenschaft abzurufen.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-118">The Property method is used to obtain a configuration object for a given property.</span></span> <span data-ttu-id="2f0f8-119">Die Optionen für das Konfigurationsobjekt sind spezifisch für den konfigurierten Typ. Isunicode ist beispielsweise nur für Zeichen folgen Eigenschaften verfügbar.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-119">The options on the configuration object are specific to the type being configured; IsUnicode is available only on string properties for example.</span></span>  

### <a name="configuring-a-primary-key"></a><span data-ttu-id="2f0f8-120">Konfigurieren eines Primärschlüssels</span><span class="sxs-lookup"><span data-stu-id="2f0f8-120">Configuring a Primary Key</span></span>  

<span data-ttu-id="2f0f8-121">Die Entity Framework Konvention für Primärschlüssel lautet wie folgt:</span><span class="sxs-lookup"><span data-stu-id="2f0f8-121">The Entity Framework convention for primary keys is:</span></span>  

1. <span data-ttu-id="2f0f8-122">Ihre Klasse definiert eine Eigenschaft mit dem Namen "ID" oder "ID".</span><span class="sxs-lookup"><span data-stu-id="2f0f8-122">Your class defines a property whose name is “ID” or “Id”</span></span>  
2. <span data-ttu-id="2f0f8-123">oder einen Klassennamen, gefolgt von "ID" oder "ID".</span><span class="sxs-lookup"><span data-stu-id="2f0f8-123">or a class name followed by “ID” or “Id”</span></span>  

<span data-ttu-id="2f0f8-124">Um explizit eine Eigenschaft als Primärschlüssel festzulegen, können Sie die Haskey-Methode verwenden.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-124">To explicitly set a property to be a primary key, you can use the HasKey method.</span></span> <span data-ttu-id="2f0f8-125">Im folgenden Beispiel wird die Haskey-Methode verwendet, um den "InstructorID"-Primärschlüssel für den officezuweisungstyp zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-125">In the following example, the HasKey method is used to configure the InstructorID primary key on the OfficeAssignment type.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a><span data-ttu-id="2f0f8-126">Konfigurieren eines zusammengesetzten Primärschlüssels</span><span class="sxs-lookup"><span data-stu-id="2f0f8-126">Configuring a Composite Primary Key</span></span>  

<span data-ttu-id="2f0f8-127">Im folgenden Beispiel werden die Eigenschaften "DepartmentID" und "Name" als zusammengesetzter Primärschlüssel des Abteilungs Typs konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-127">The following example configures the DepartmentID and Name properties to be the composite primary key of the Department type.</span></span>  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a><span data-ttu-id="2f0f8-128">Ausschalten der Identität für numerische Primärschlüssel</span><span class="sxs-lookup"><span data-stu-id="2f0f8-128">Switching off Identity for Numeric Primary Keys</span></span>  

<span data-ttu-id="2f0f8-129">Im folgenden Beispiel wird die DepartmentID-Eigenschaft auf System. ComponentModel. DataAnnotations. databasegeneratedoption. None festgelegt, um anzugeben, dass der Wert nicht von der Datenbank generiert wird.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-129">The following example sets the DepartmentID property to System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None to indicate that the value will not be generated by the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a><span data-ttu-id="2f0f8-130">Angeben der maximalen Länge für eine Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="2f0f8-130">Specifying the Maximum Length on a Property</span></span>  

<span data-ttu-id="2f0f8-131">Im folgenden Beispiel sollte die Name-Eigenschaft nicht länger als 50 Zeichen sein.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-131">In the following example, the Name property should be no longer than 50 characters.</span></span> <span data-ttu-id="2f0f8-132">Wenn Sie den Wert länger als 50 Zeichen machen, wird eine [dbentityvalidationexception](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) -Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-132">If you make the value longer than 50 characters, you will get a [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) exception.</span></span> <span data-ttu-id="2f0f8-133">Wenn Code First eine Datenbank aus diesem Modell erstellt, wird auch die maximale Länge der Spalte Name auf 50 Zeichen festgelegt.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-133">If Code First creates a database from this model it will also set the maximum length of the Name column to 50 characters.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a><span data-ttu-id="2f0f8-134">Konfigurieren der Eigenschaft als erforderlich</span><span class="sxs-lookup"><span data-stu-id="2f0f8-134">Configuring the Property to be Required</span></span>  

<span data-ttu-id="2f0f8-135">Im folgenden Beispiel ist die Name-Eigenschaft erforderlich.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-135">In the following example, the Name property is required.</span></span> <span data-ttu-id="2f0f8-136">Wenn Sie den Namen nicht angeben, wird eine dbentityvalidationexception-Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-136">If you do not specify the Name, you will get a DbEntityValidationException exception.</span></span> <span data-ttu-id="2f0f8-137">Wenn Code First eine Datenbank aus diesem Modell erstellt, ist die Spalte, die zum Speichern dieser Eigenschaft verwendet wird, in der Regel keine NULL-Werte zulässig.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-137">If Code First creates a database from this model then the column used to store this property will usually be non-nullable.</span></span>  

> [!NOTE]
> <span data-ttu-id="2f0f8-138">In einigen Fällen ist es möglicherweise nicht möglich, dass die Spalte in der Datenbank keine NULL-Werte zulässt, auch wenn die-Eigenschaft erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-138">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="2f0f8-139">Wenn z. b. eine TPH-Vererbungs Strategie verwendet wird, werden Daten für mehrere Typen in einer einzelnen Tabelle gespeichert.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-139">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="2f0f8-140">Wenn ein abgeleiteter Typ eine erforderliche Eigenschaft enthält, kann die Spalte nicht auf NULL festleg Bare Werte festgelegt werden, da nicht alle Typen in der Hierarchie über diese Eigenschaft verfügen.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-140">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a><span data-ttu-id="2f0f8-141">Konfigurieren eines Indexes für eine oder mehrere Eigenschaften</span><span class="sxs-lookup"><span data-stu-id="2f0f8-141">Configuring an Index on one or more properties</span></span>  

> [!NOTE]
> <span data-ttu-id="2f0f8-142">**Nur ab EF 6.1** : das Index Attribut wurde in Entity Framework 6,1 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-142">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="2f0f8-143">Wenn Sie eine frühere Version verwenden, gelten die Informationen in diesem Abschnitt nicht.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-143">If you are using an earlier version the information in this section does not apply.</span></span>  

<span data-ttu-id="2f0f8-144">Das Erstellen von Indizes wird von der fließenden API nicht nativ unterstützt, aber Sie können die Unterstützung von **Indexattribute** über die fließende API nutzen.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-144">Creating indexes isn't natively supported by the Fluent API, but you can make use of the support for **IndexAttribute** via the Fluent API.</span></span> <span data-ttu-id="2f0f8-145">Index Attribute werden verarbeitet, indem eine Modell Anmerkung in das Modell eingeschlossen wird, die dann später in der Pipeline in einen Index in der Datenbank umgewandelt wird.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-145">Index attributes are processed by including a model annotation on the model that is then turned into an Index in the database later in the pipeline.</span></span> <span data-ttu-id="2f0f8-146">Sie können dieselben Anmerkungen mithilfe der fließend-API manuell hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-146">You can manually add these same annotations using the Fluent API.</span></span>  

<span data-ttu-id="2f0f8-147">Die einfachste Möglichkeit hierfür ist das Erstellen einer Instanz von **Indexattribute** , die alle Einstellungen für den neuen Index enthält.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-147">The easiest way to do this is to create an instance of **IndexAttribute** that contains all the settings for the new index.</span></span> <span data-ttu-id="2f0f8-148">Anschließend können Sie eine Instanz der **Indexnotation** erstellen, bei der es sich um einen EF-spezifischen Typ handelt, der die **Indexattribute** -Einstellungen in eine Modell Anmerkung konvertiert, die im EF-Modell gespeichert werden kann.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-148">You can then create an instance of **IndexAnnotation** which is an EF specific type that will convert the **IndexAttribute** settings into a model annotation that can be stored on the EF model.</span></span> <span data-ttu-id="2f0f8-149">Diese können dann an die **hascolumnannotation** -Methode für die fließende API weitergegeben werden, wobei der namens **Index** für die Anmerkung angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-149">These can then be passed to the **HasColumnAnnotation** method on the Fluent API, specifying the name **Index** for the annotation.</span></span>  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

<span data-ttu-id="2f0f8-150">Eine umfassende Liste der in " **Indexattribute**" verfügbaren Einstellungen finden Sie im Abschnitt " *Index* " der [Code First Daten Anmerkungen](~/ef6/modeling/code-first/data-annotations.md).</span><span class="sxs-lookup"><span data-stu-id="2f0f8-150">For a complete list of the settings available in **IndexAttribute**, see the *Index* section of [Code First Data Annotations](~/ef6/modeling/code-first/data-annotations.md).</span></span> <span data-ttu-id="2f0f8-151">Dies umfasst das Anpassen des Index namens, das Erstellen eindeutiger Indizes und das Erstellen von mehrspaltigen Indizes.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-151">This includes customizing the index name, creating unique indexes, and creating multi-column indexes.</span></span>  

<span data-ttu-id="2f0f8-152">Sie können mehrere Index Anmerkungen für eine einzelne Eigenschaft angeben, indem Sie ein Array von " **Indexattribute** " an den Konstruktor von " **indexannotation**" übergeben.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-152">You can specify multiple index annotations on a single property by passing an array of **IndexAttribute** to the constructor of **IndexAnnotation**.</span></span>  

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

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a><span data-ttu-id="2f0f8-153">Angeben, dass eine CLR-Eigenschaft einer Spalte in der Datenbank nicht zugeordnet werden soll</span><span class="sxs-lookup"><span data-stu-id="2f0f8-153">Specifying Not to Map a CLR Property to a Column in the Database</span></span>  

<span data-ttu-id="2f0f8-154">Im folgenden Beispiel wird gezeigt, wie angegeben wird, dass eine Eigenschaft eines CLR-Typs keiner Spalte in der Datenbank zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-154">The following example shows how to specify that a property on a CLR type is not mapped to a column in the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a><span data-ttu-id="2f0f8-155">Mapping einer CLR-Eigenschaft zu einer bestimmten Spalte in der Datenbank</span><span class="sxs-lookup"><span data-stu-id="2f0f8-155">Mapping a CLR Property to a Specific Column in the Database</span></span>  

<span data-ttu-id="2f0f8-156">Im folgenden Beispiel wird die Name CLR-Eigenschaft der DepartmentName-Daten Bank Spalte zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-156">The following example maps the Name CLR property to the DepartmentName database column.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="2f0f8-157">Umbenennen eines fremd Schlüssels, der nicht im Modell definiert ist</span><span class="sxs-lookup"><span data-stu-id="2f0f8-157">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="2f0f8-158">Wenn Sie keinen Fremdschlüssel für einen CLR-Typ definieren, aber angeben möchten, welcher Name in der Datenbank enthalten sein soll, gehen Sie wie folgt vor:</span><span class="sxs-lookup"><span data-stu-id="2f0f8-158">If you choose not to define a foreign key on a CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a><span data-ttu-id="2f0f8-159">Konfigurieren, ob eine Zeichen folgen Eigenschaft Unicode-Inhalt unterstützt</span><span class="sxs-lookup"><span data-stu-id="2f0f8-159">Configuring whether a String Property Supports Unicode Content</span></span>  

<span data-ttu-id="2f0f8-160">Standardmäßig sind Zeichen folgen Unicode (nvarchar in SQL Server).</span><span class="sxs-lookup"><span data-stu-id="2f0f8-160">By default strings are Unicode (nvarchar in SQL Server).</span></span> <span data-ttu-id="2f0f8-161">Sie können die isunicode-Methode verwenden, um anzugeben, dass eine Zeichenfolge vom Typ varchar sein soll.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-161">You can use the IsUnicode method to specify that a string should be of varchar type.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a><span data-ttu-id="2f0f8-162">Konfigurieren des Datentyps einer Daten Bank Spalte</span><span class="sxs-lookup"><span data-stu-id="2f0f8-162">Configuring the Data Type of a Database Column</span></span>  

<span data-ttu-id="2f0f8-163">Die [hascolumntype](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) -Methode ermöglicht die Zuordnung zu verschiedenen Darstellungen desselben Basistyp.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-163">The [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) method enables mapping to different representations of the same basic type.</span></span> <span data-ttu-id="2f0f8-164">Wenn Sie diese Methode verwenden, können Sie die Daten nicht zur Laufzeit konvertieren.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-164">Using this method does not enable you to perform any conversion of the data at run time.</span></span> <span data-ttu-id="2f0f8-165">Beachten Sie, dass isunicode die bevorzugte Methode zum Festlegen von Spalten auf varchar ist, da dies Daten Bank agnostisch ist.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-165">Note that IsUnicode is the preferred way of setting columns to varchar, as it is database agnostic.</span></span>  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a><span data-ttu-id="2f0f8-166">Konfigurieren von Eigenschaften für einen komplexen Typ</span><span class="sxs-lookup"><span data-stu-id="2f0f8-166">Configuring Properties on a Complex Type</span></span>  

<span data-ttu-id="2f0f8-167">Es gibt zwei Möglichkeiten, skalare Eigenschaften für einen komplexen Typ zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-167">There are two ways to configure scalar properties on a complex type.</span></span>  

<span data-ttu-id="2f0f8-168">Sie können die-Eigenschaft für complextypeconfiguration aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-168">You can call Property on ComplexTypeConfiguration.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

<span data-ttu-id="2f0f8-169">Sie können auch die Punkt Notation verwenden, um auf eine Eigenschaft eines komplexen Typs zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-169">You can also use the dot notation to access a property of a complex type.</span></span>  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a><span data-ttu-id="2f0f8-170">Konfigurieren einer Eigenschaft, die als Token für die vollständige Parallelität verwendet werden soll</span><span class="sxs-lookup"><span data-stu-id="2f0f8-170">Configuring a Property to Be Used as an Optimistic Concurrency Token</span></span>  

<span data-ttu-id="2f0f8-171">Um anzugeben, dass eine Eigenschaft in einer Entität ein Parallelitäts Token darstellt, können Sie entweder das Attribut "-Attribut" oder die iskonaccesscytoken-Methode verwenden.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-171">To specify that a property in an entity represents a concurrency token, you can use either the ConcurrencyCheck attribute or the IsConcurrencyToken method.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

<span data-ttu-id="2f0f8-172">Sie können auch die IsRowVersion-Methode verwenden, um die Eigenschaft als Zeilen Version in der Datenbank zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-172">You can also use the IsRowVersion method to configure the property to be a row version in the database.</span></span> <span data-ttu-id="2f0f8-173">Wenn die Eigenschaft auf eine Zeilen Version festgelegt wird, wird Sie automatisch als Token für die vollständige Parallelität konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-173">Setting the property to be a row version automatically configures it to be an optimistic concurrency token.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a><span data-ttu-id="2f0f8-174">Typzuordnung</span><span class="sxs-lookup"><span data-stu-id="2f0f8-174">Type Mapping</span></span>  

### <a name="specifying-that-a-class-is-a-complex-type"></a><span data-ttu-id="2f0f8-175">Angeben, dass eine Klasse ein komplexer Typ ist</span><span class="sxs-lookup"><span data-stu-id="2f0f8-175">Specifying That a Class Is a Complex Type</span></span>  

<span data-ttu-id="2f0f8-176">Gemäß der Konvention wird ein Typ, der keinen Primärschlüssel enthält, als komplexer Typ behandelt.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-176">By convention, a type that has no primary key specified is treated as a complex type.</span></span> <span data-ttu-id="2f0f8-177">Es gibt einige Szenarien, in denen Code First einen komplexen Typ nicht erkennt (wenn Sie z. b. über eine Eigenschaft mit dem Namen "ID" verfügen, aber nicht bedeuten, dass es sich um einen Primärschlüssel handelt).</span><span class="sxs-lookup"><span data-stu-id="2f0f8-177">There are some scenarios where Code First will not detect a complex type (for example, if you do have a property called ID, but you do not mean for it to be a primary key).</span></span> <span data-ttu-id="2f0f8-178">In solchen Fällen verwenden Sie die fließende API, um explizit anzugeben, dass ein Typ ein komplexer Typ ist.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-178">In such cases, you would use the fluent API to explicitly specify that a type is a complex type.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a><span data-ttu-id="2f0f8-179">Angeben, dass kein CLR-Entitätstyp einer Tabelle in der Datenbank zugeordnet werden soll</span><span class="sxs-lookup"><span data-stu-id="2f0f8-179">Specifying Not to Map a CLR Entity Type to a Table in the Database</span></span>  

<span data-ttu-id="2f0f8-180">Im folgenden Beispiel wird gezeigt, wie ein CLR-Typ von der Zuordnung zu einer Tabelle in der-Datenbank ausgeschlossen wird.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-180">The following example shows how to exclude a CLR type from being mapped to a table in the database.</span></span>  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a><span data-ttu-id="2f0f8-181">Zuordnung eines Entitäts Typs zu einer bestimmten Tabelle in der Datenbank</span><span class="sxs-lookup"><span data-stu-id="2f0f8-181">Mapping an Entity Type to a Specific Table in the Database</span></span>  

<span data-ttu-id="2f0f8-182">Alle Eigenschaften der Abteilung werden Spalten in einer Tabelle mit dem Namen T_ Abteilung zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-182">All properties of Department will be mapped to columns in a table called t_ Department.</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

<span data-ttu-id="2f0f8-183">Sie können auch den Schema Namen wie folgt angeben:</span><span class="sxs-lookup"><span data-stu-id="2f0f8-183">You can also specify the schema name like this:</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a><span data-ttu-id="2f0f8-184">Zuordnung der "Tabelle pro Hierarchie"-Vererbung (TPH)</span><span class="sxs-lookup"><span data-stu-id="2f0f8-184">Mapping the Table-Per-Hierarchy (TPH) Inheritance</span></span>  

<span data-ttu-id="2f0f8-185">Im TPH-Zuordnungsszenario werden alle Typen in einer Vererbungs Hierarchie einer einzelnen Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-185">In the TPH mapping scenario, all types in an inheritance hierarchy are mapped to a single table.</span></span> <span data-ttu-id="2f0f8-186">Eine diskriminatorspalte wird verwendet, um den Typ der einzelnen Zeilen zu identifizieren.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-186">A discriminator column is used to identify the type of each row.</span></span> <span data-ttu-id="2f0f8-187">Wenn Sie Ihr Modell mit Code First erstellen, ist TPH die Standardstrategie für die Typen, die an der Vererbungs Hierarchie teilnehmen.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-187">When creating your model with Code First, TPH is the default strategy for the types that participate in the inheritance hierarchy.</span></span> <span data-ttu-id="2f0f8-188">Standardmäßig wird die diskriminatorspalte der Tabelle mit dem Namen "Diskriminator" hinzugefügt, und der CLR-Typname jedes Typs in der Hierarchie wird für die diskriminatorwerte verwendet.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-188">By default, the discriminator column is added to the table with the name “Discriminator” and the CLR type name of each type in the hierarchy is used for the discriminator values.</span></span> <span data-ttu-id="2f0f8-189">Sie können das Standardverhalten mithilfe der flüssigen API ändern.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-189">You can modify the default behavior by using the fluent API.</span></span>  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a><span data-ttu-id="2f0f8-190">Zuordnung der "Tabelle pro Typ"-Vererbung (TPT)</span><span class="sxs-lookup"><span data-stu-id="2f0f8-190">Mapping the Table-Per-Type (TPT) Inheritance</span></span>  

<span data-ttu-id="2f0f8-191">Im TPT-Zuordnungsszenario werden alle Typen einzelnen Tabellen zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-191">In the TPT mapping scenario, all types are mapped to individual tables.</span></span> <span data-ttu-id="2f0f8-192">Eigenschaften, die nur zu einem Basistyp oder einem abgeleiteten Typ gehören, werden in einer Tabelle gespeichert, die diesem Typ zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-192">Properties that belong solely to a base type or derived type are stored in a table that maps to that type.</span></span> <span data-ttu-id="2f0f8-193">Tabellen, die abgeleiteten Typen zugeordnet sind, speichern auch einen Fremdschlüssel, der die abgeleitete Tabelle mit der Basistabelle verbindet.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-193">Tables that map to derived types also store a foreign key that joins the derived table with the base table.</span></span>  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a><span data-ttu-id="2f0f8-194">Zuordnung der TPC-Vererbung (Tabelle pro konkrete Klasse)</span><span class="sxs-lookup"><span data-stu-id="2f0f8-194">Mapping the Table-Per-Concrete Class (TPC) Inheritance</span></span>  

<span data-ttu-id="2f0f8-195">Im TPC-Zuordnungsszenario werden alle nicht abstrakten Typen in der Hierarchie einzelnen Tabellen zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-195">In the TPC mapping scenario, all non-abstract types in the hierarchy are mapped to individual tables.</span></span> <span data-ttu-id="2f0f8-196">Die Tabellen, die den abgeleiteten Klassen zugeordnet sind, haben keine Beziehung zu der Tabelle, die der Basisklasse in der Datenbank zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-196">The tables that map to the derived classes have no relationship to the table that maps to the base class in the database.</span></span> <span data-ttu-id="2f0f8-197">Alle Eigenschaften einer Klasse, einschließlich der geerbten Eigenschaften, werden den Spalten der entsprechenden Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-197">All properties of a class, including inherited properties, are mapped to columns of the corresponding table.</span></span>  

<span data-ttu-id="2f0f8-198">Ruft die mapinheritedproperties-Methode auf, um jeden abgeleiteten Typ zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-198">Call the MapInheritedProperties method to configure each derived type.</span></span> <span data-ttu-id="2f0f8-199">Mapinheritedproperties ordnet alle Eigenschaften, die von der Basisklasse geerbt wurden, neuen Spalten in der Tabelle für die abgeleitete Klasse zu.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-199">MapInheritedProperties remaps all properties that were inherited from the base class to new columns in the table for the derived class.</span></span>  

> [!NOTE]
> <span data-ttu-id="2f0f8-200">Beachten Sie Folgendes: da die an der TPC-Vererbungs Hierarchie beteiligten Tabellen keinen Primärschlüssel gemeinsam verwenden, gibt es beim Einfügen in Tabellen, die Unterklassen zugeordnet sind, doppelte Entitäts Schlüssel, wenn von der Datenbank generierte Werte mit dem gleichen identitätsseed vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-200">Note that because the tables participating in TPC inheritance hierarchy do not share a primary key there will be duplicate entity keys when inserting in tables that are mapped to subclasses if you have database generated values with the same identity seed.</span></span> <span data-ttu-id="2f0f8-201">Um dieses Problem zu beheben, können Sie entweder für jede Tabelle einen anderen ursprünglichen Startwert angeben oder die Identität der Primärschlüssel Eigenschaft ausschalten.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-201">To solve this problem you can either specify a different initial seed value for each table or switch off identity on the primary key property.</span></span> <span data-ttu-id="2f0f8-202">Identity ist der Standardwert für ganzzahlige Schlüsseleigenschaften beim Arbeiten mit Code First.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-202">Identity is the default value for integer key properties when working with Code First.</span></span>  

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

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a><span data-ttu-id="2f0f8-203">Zuordnung von Eigenschaften eines Entitäts Typs zu mehreren Tabellen in der Datenbank (Entitäts Aufteilung)</span><span class="sxs-lookup"><span data-stu-id="2f0f8-203">Mapping Properties of an Entity Type to Multiple Tables in the Database (Entity Splitting)</span></span>  

<span data-ttu-id="2f0f8-204">Durch die Entitäts Aufteilung können die Eigenschaften eines Entitäts Typs auf mehrere Tabellen verteilt werden.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-204">Entity splitting allows the properties of an entity type to be spread across multiple tables.</span></span> <span data-ttu-id="2f0f8-205">Im folgenden Beispiel ist die Abteilungs Entität in zwei Tabellen aufgeteilt: Department und departmentdetails.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-205">In the following example, the Department entity is split into two tables: Department and DepartmentDetails.</span></span> <span data-ttu-id="2f0f8-206">Bei der Entitäts Aufteilung werden mehrere Aufrufe der Map-Methode verwendet, um einer bestimmten Tabelle eine Teilmenge von Eigenschaften zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-206">Entity splitting uses multiple calls to the Map method to map a subset of properties to a specific table.</span></span>  

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

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a><span data-ttu-id="2f0f8-207">Zuordnung mehrerer Entitäts Typen zu einer Tabelle in der Datenbank (Tabellen Aufteilung)</span><span class="sxs-lookup"><span data-stu-id="2f0f8-207">Mapping Multiple Entity Types to One Table in the Database (Table Splitting)</span></span>  

<span data-ttu-id="2f0f8-208">Im folgenden Beispiel werden zwei Entitäts Typen, die einen Primärschlüssel gemeinsam verwenden, einer Tabelle zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-208">The following example maps two entity types that share a primary key to one table.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a><span data-ttu-id="2f0f8-209">Zuordnung eines Entitäts Typs zu gespeicherten Prozeduren zum Einfügen/Aktualisieren/Löschen (EF6 oder höher)</span><span class="sxs-lookup"><span data-stu-id="2f0f8-209">Mapping an Entity Type to Insert/Update/Delete Stored Procedures (EF6 onwards)</span></span>  

<span data-ttu-id="2f0f8-210">Beginnend mit EF6 können Sie eine Entität zuordnen, um gespeicherte Prozeduren für Insert Update und DELETE zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-210">Starting with EF6 you can map an entity to use stored procedures for insert update and delete.</span></span> <span data-ttu-id="2f0f8-211">Weitere Informationen finden Sie unter [Code First gespeicherten Prozeduren INSERT/UPDATE/DELETE](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span><span class="sxs-lookup"><span data-stu-id="2f0f8-211">For more details see, [Code First Insert/Update/Delete Stored Procedures](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span></span>  

## <a name="model-used-in-samples"></a><span data-ttu-id="2f0f8-212">In Beispielen verwendetes Modell</span><span class="sxs-lookup"><span data-stu-id="2f0f8-212">Model Used in Samples</span></span>  

<span data-ttu-id="2f0f8-213">Das folgende Code First Modell wird für die Beispiele auf dieser Seite verwendet.</span><span class="sxs-lookup"><span data-stu-id="2f0f8-213">The following Code First model is used for the samples on this page.</span></span>  

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
