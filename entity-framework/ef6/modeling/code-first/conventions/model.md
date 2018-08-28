---
title: Modellbasierten Konventionen - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: c1f416ba599445c07bd946714e9b6208b62f5951
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996946"
---
# <a name="model-based-conventions"></a><span data-ttu-id="e3125-102">Modellbasierten Konventionen</span><span class="sxs-lookup"><span data-stu-id="e3125-102">Model-Based Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="e3125-103">**Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e3125-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="e3125-104">Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="e3125-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="e3125-105">Modellbasierter Konventionen sind eine erweiterte Konfiguration für das konventionsbasierten Methode.</span><span class="sxs-lookup"><span data-stu-id="e3125-105">Model based conventions are an advanced method of convention based model configuration.</span></span> <span data-ttu-id="e3125-106">In den meisten Szenarien die [benutzerdefinierte Code First-Konvention-API auf DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="e3125-106">For most scenarios the [Custom Code First Convention API on DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) should be used.</span></span> <span data-ttu-id="e3125-107">Ein Überblick über den DbModelBuilder-API für Konventionen wird empfohlen, vor der Verwendung von Konventionen von Anwendungsmodellen basiert.</span><span class="sxs-lookup"><span data-stu-id="e3125-107">An understanding of the DbModelBuilder API for conventions is recommended before using model based conventions.</span></span>  

<span data-ttu-id="e3125-108">Konventionen von Anwendungsmodellen basierend ermöglichen die Erstellung von Konventionen, die Auswirkungen auf die Eigenschaften und Tabellen, die nicht über die Standardkonventionen konfigurierbar sind.</span><span class="sxs-lookup"><span data-stu-id="e3125-108">Model based conventions allow the creation of conventions that affect properties and tables which are not configurable through standard conventions.</span></span> <span data-ttu-id="e3125-109">Beispiele hierfür sind Diskriminator in Tabelle pro Hierarchie Modelle und unabhängige Zuordnung Spalten.</span><span class="sxs-lookup"><span data-stu-id="e3125-109">Examples of these are discriminator columns in table per hierarchy models and Independent Association columns.</span></span>  

## <a name="creating-a-convention"></a><span data-ttu-id="e3125-110">Erstellen eine Konvention</span><span class="sxs-lookup"><span data-stu-id="e3125-110">Creating a Convention</span></span>   

<span data-ttu-id="e3125-111">Wenn in der Pipeline die Konvention für das Modell angewendet werden muss, ist der erste Schritt beim Erstellen einer Konvention basierendes Modell auswählen.</span><span class="sxs-lookup"><span data-stu-id="e3125-111">The first step in creating a model based convention is choosing when in the pipeline the convention needs to be applied to the model.</span></span> <span data-ttu-id="e3125-112">Es gibt zwei Arten von Konventionen von Anwendungsmodellen, Konzept-(C-Bereich) und Store (S-Bereich) aus.</span><span class="sxs-lookup"><span data-stu-id="e3125-112">There are two types of model conventions, Conceptual (C-Space) and Store (S-Space).</span></span> <span data-ttu-id="e3125-113">Eine C-Space-Konvention wird für das Modell angewendet, die die Anwendung erstellt wird, während eine S-Space-Konvention auf die Version des Modells angewendet wird, die die Datenbank darstellt und Steuerelemente, beispielsweise wie automatisch generierten Spalten heißen.</span><span class="sxs-lookup"><span data-stu-id="e3125-113">A C-Space convention is applied to the model that the application builds, whereas an S-Space convention is applied to the version of the model that represents the database and controls things such as how automatically-generated columns are named.</span></span>  

<span data-ttu-id="e3125-114">Eine modellkonvention ist eine Klasse, die von IConceptualModelConvention oder IStoreModelConvention erweitert.</span><span class="sxs-lookup"><span data-stu-id="e3125-114">A model convention is a class that extends from either IConceptualModelConvention or IStoreModelConvention.</span></span>  <span data-ttu-id="e3125-115">Diese Schnittstellen, die beide einen generischen Typ akzeptieren, die von können geben MetadataItem die verwendet wird, um den Datentyp zu filtern, dem für die Konvention gilt.</span><span class="sxs-lookup"><span data-stu-id="e3125-115">These interfaces both accept a generic type that can be of type MetadataItem which is used to filter the data type that the convention applies to.</span></span>  

## <a name="adding-a-convention"></a><span data-ttu-id="e3125-116">Hinzufügen einer Konvention</span><span class="sxs-lookup"><span data-stu-id="e3125-116">Adding a Convention</span></span>   

<span data-ttu-id="e3125-117">Konventionen von Anwendungsmodellen werden in die gleiche Weise wie reguläre Konventionen Klassen hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e3125-117">Model conventions are added in the same way as regular conventions classes.</span></span> <span data-ttu-id="e3125-118">In der **"onmodelcreating"** -Methode, die Konvention der Liste mit den Konventionen für ein Modell hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="e3125-118">In the **OnModelCreating** method, add the convention to the list of conventions for a model.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

public class BlogContext : DbContext  
{  
    public DbSet<Post> Posts { get; set; }  
    public DbSet<Comment> Comments { get; set; }  

    protected override void OnModelCreating(DbModelBuilder modelBuilder)  
    {  
        modelBuilder.Conventions.Add<MyModelBasedConvention>();  
    }  
}
```  

<span data-ttu-id="e3125-119">Eine Konvention kann auch hinzugefügt werden, in Bezug auf eine andere Konvention, die mithilfe der Conventions.AddBefore\< \> oder Conventions.AddAfter\< \> Methoden.</span><span class="sxs-lookup"><span data-stu-id="e3125-119">A convention can also be added in relation to another convention using the Conventions.AddBefore\<\> or Conventions.AddAfter\<\> methods.</span></span> <span data-ttu-id="e3125-120">Weitere Informationen zu den Konventionen, die Entity Framework gilt finden Sie im Abschnitt Hinweise.</span><span class="sxs-lookup"><span data-stu-id="e3125-120">For more information about the conventions that Entity Framework applies see the notes section.</span></span>  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a><span data-ttu-id="e3125-121">Beispiel: Diskriminator-Modellkonvention</span><span class="sxs-lookup"><span data-stu-id="e3125-121">Example: Discriminator Model Convention</span></span>  

<span data-ttu-id="e3125-122">Umbenennen von Spalten, die von EF generierten ist ein Beispiel von etwas, das mit der andere für APIs nicht möglich.</span><span class="sxs-lookup"><span data-stu-id="e3125-122">Renaming columns generated by EF is an example of something that you can’t do with the other conventions APIs.</span></span>  <span data-ttu-id="e3125-123">Dies ist eine Situation mit Konventionen von Anwendungsmodellen Ihre einzige Option ist.</span><span class="sxs-lookup"><span data-stu-id="e3125-123">This is a situation where using model conventions is your only option.</span></span>  

<span data-ttu-id="e3125-124">Verdeutlicht, wie Sie eine Konvention basierendes Modell verwenden, um generierten Spalten zu konfigurieren ist die Methode anpassen, die als Unterscheidungsspalte Spalten identisch sind.</span><span class="sxs-lookup"><span data-stu-id="e3125-124">An example of how to use a model based convention to configure generated columns is customizing the way discriminator columns are named.</span></span>  <span data-ttu-id="e3125-125">Im folgenden finden Sie ein Beispiel für eine einfache Modellbasierter-Konvention, die jede Spalte im Modell mit dem Namen "Discriminator" an "EntityType" umbenannt.</span><span class="sxs-lookup"><span data-stu-id="e3125-125">Below is an example of a simple model based convention that renames every column in the model named “Discriminator” to “EntityType”.</span></span>  <span data-ttu-id="e3125-126">Dies schließt Spalten an, dass der Entwickler einfach mit der Bezeichnung "Discriminator genannt".</span><span class="sxs-lookup"><span data-stu-id="e3125-126">This includes columns that the developer simply named “Discriminator”.</span></span> <span data-ttu-id="e3125-127">Da die Spalte "Discriminator" genannt. eine generierte Spalte ist muss dies im S-Bereich ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="e3125-127">Since the “Discriminator” column is a generated column this needs to run in S-Space.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

class DiscriminatorRenamingConvention : IStoreModelConvention<EdmProperty>  
{  
    public void Apply(EdmProperty property, DbModel model)  
    {            
        if (property.Name == "Discriminator")  
        {  
            property.Name = "EntityType";  
        }  
    }  
}
```  

## <a name="example-general-ia-renaming-convention"></a><span data-ttu-id="e3125-128">Beispiel: Allgemeine IA-Konvention umbenennen</span><span class="sxs-lookup"><span data-stu-id="e3125-128">Example: General IA Renaming Convention</span></span>   

<span data-ttu-id="e3125-129">Ein weiteres etwas komplizierteres Beispiel basierendes modellkonventionen in Aktion ist können Sie konfigurieren, dass unabhängige Zuordnungen (IAs) benannt werden.</span><span class="sxs-lookup"><span data-stu-id="e3125-129">Another more complicated example of model based conventions in action is to configure the way that Independent Associations (IAs) are named.</span></span>  <span data-ttu-id="e3125-130">Dies ist eine Situation, in denen Konventionen von Anwendungsmodellen anwendbar sind, da IAs von EF generiert werden, und sind nicht vorhanden ist, im Modell, das von der DbModelBuilder-API zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="e3125-130">This is a situation where Model conventions are applicable because IAs are generated by EF and aren’t present in the model that the DbModelBuilder API can access.</span></span>  

<span data-ttu-id="e3125-131">Wenn EF eine IA generiert, wird eine Spalte namens EntityType_KeyName.</span><span class="sxs-lookup"><span data-stu-id="e3125-131">When EF generates an IA, it creates a column named EntityType_KeyName.</span></span> <span data-ttu-id="e3125-132">Z. B. mit dem Namen für eine Zuordnung mit dem Namen Customer, mit einer Schlüsselspalte "CustomerID", die sie eine Spalte namens Customer_CustomerId generieren würden.</span><span class="sxs-lookup"><span data-stu-id="e3125-132">For example, for an association named Customer with a key column named CustomerId it would generate a column named Customer_CustomerId.</span></span> <span data-ttu-id="e3125-133">Die folgende Konvention Streifen der "\_'-Zeichen aus den Namen der Spalte, die für den IA generiert wird</span><span class="sxs-lookup"><span data-stu-id="e3125-133">The following convention strips the ‘\_’ character out of the column name that is generated for the IA.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

// Provides a convention for fixing the independent association (IA) foreign key column names.  
public class ForeignKeyNamingConvention : IStoreModelConvention<AssociationType>
{

    public void Apply(AssociationType association, DbModel model)
    {
        // Identify ForeignKey properties (including IAs)  
        if (association.IsForeignKey)
        {
            // rename FK columns  
            var constraint = association.Constraint;
            if (DoPropertiesHaveDefaultNames(constraint.FromProperties, constraint.ToRole.Name, constraint.ToProperties))
            {
                NormalizeForeignKeyProperties(constraint.FromProperties);
            }
            if (DoPropertiesHaveDefaultNames(constraint.ToProperties, constraint.FromRole.Name, constraint.FromProperties))
            {
                NormalizeForeignKeyProperties(constraint.ToProperties);
            }
        }
    }

    private bool DoPropertiesHaveDefaultNames(ReadOnlyMetadataCollection<EdmProperty> properties, string roleName, ReadOnlyMetadataCollection<EdmProperty> otherEndProperties)
    {
        if (properties.Count != otherEndProperties.Count)
        {
            return false;
        }

        for (int i = 0; i < properties.Count; ++i)
        {
            if (!properties[i].Name.EndsWith("_" + otherEndProperties[i].Name))
            {
                return false;
            }
        }
        return true;
    }

    private void NormalizeForeignKeyProperties(ReadOnlyMetadataCollection<EdmProperty> properties)
    {
        for (int i = 0; i \< properties.Count; ++i)
        {
            int underscoreIndex = properties[i].Name.IndexOf('_');
            if (underscoreIndex > 0)
            {
                properties[i].Name = properties[i].Name.Remove(underscoreIndex, 1);
            }                 
        }
    }
}
```  

## <a name="extending-existing-conventions"></a><span data-ttu-id="e3125-134">Erweitern Sie vorhandene Konventionen</span><span class="sxs-lookup"><span data-stu-id="e3125-134">Extending Existing Conventions</span></span>   

<span data-ttu-id="e3125-135">Falls also eine Konvention zu schreiben, die einem der Konventionen entspricht, die Entity Framework bereits auf das Modell angewendet wird, können Sie immer diese Konvention zur Vermeidung von Grund auf neu schreiben müssen, erweitern.</span><span class="sxs-lookup"><span data-stu-id="e3125-135">If you need to write a convention that is similar to one of the conventions that Entity Framework already applies to your model you can always extend that convention to avoid having to rewrite it from scratch.</span></span>  <span data-ttu-id="e3125-136">Ein Beispiel hierfür ist, Ersetzen der vorhandene Id, die, die Konvention, die durch eine benutzerdefinierte.</span><span class="sxs-lookup"><span data-stu-id="e3125-136">An example of this is to replace the existing Id matching convention with a custom one.</span></span>   <span data-ttu-id="e3125-137">Ein weiterer Vorteil ist, überschreiben die wichtigsten Konvention ist, dass die überschriebene Methode aufgerufen wird nur dann, wenn kein Schlüssel bereits erkannt oder explizit konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="e3125-137">An added benefit to overriding the key convention is that the overridden method will get called only if there is no key already detected or explicitly configured.</span></span> <span data-ttu-id="e3125-138">Eine Liste von Konventionen, die von Entity Framework verwendet ist hier verfügbar: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="e3125-138">A list of conventions that used by Entity Framework is available here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;
using System.Linq;  

// Convention to detect primary key properties.
// Recognized naming patterns in order of precedence are:
// 1. 'Key'
// 2. [type name]Key
// Primary key detection is case insensitive.
public class CustomKeyDiscoveryConvention : KeyDiscoveryConvention
{
    private const string Id = "Key";

    protected override IEnumerable<EdmProperty> MatchKeyProperty(
        EntityType entityType, IEnumerable<EdmProperty> primitiveProperties)
    {
        Debug.Assert(entityType != null);
        Debug.Assert(primitiveProperties != null);

        var matches = primitiveProperties
            .Where(p => Id.Equals(p.Name, StringComparison.OrdinalIgnoreCase));

        if (!matches.Any())
       {
            matches = primitiveProperties
                .Where(p => (entityType.Name + Id).Equals(p.Name, StringComparison.OrdinalIgnoreCase));
        }

        // If the number of matches is more than one, then multiple properties matched differing only by
        // case--for example, "Key" and "key".  
        if (matches.Count() > 1)
        {
            throw new InvalidOperationException("Multiple properties match the key convention");
        }

        return matches;
    }
}
```  

<span data-ttu-id="e3125-139">Dann müssen wir unsere neue Konvention vor der vorhandenen Schlüssel Konvention hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="e3125-139">We then need to add our new convention before the existing key convention.</span></span> <span data-ttu-id="e3125-140">Nachdem wir die CustomKeyDiscoveryConvention hinzugefügt haben, können wir die IdKeyDiscoveryConvention entfernen.</span><span class="sxs-lookup"><span data-stu-id="e3125-140">After we add the CustomKeyDiscoveryConvention, we can remove the IdKeyDiscoveryConvention.</span></span>  <span data-ttu-id="e3125-141">Wenn wir die vorhandenen IdKeyDiscoveryConvention entfernt haben, dieser Konvention noch Vorrang vor würden die Id-Discovery-Konvention, da er ausgeführt wird, zuerst, aber in die Groß-/Kleinschreibung, keine "Key"-Eigenschaft gefunden wird, wird die Konvention "Id" ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="e3125-141">If we didn’t remove the existing IdKeyDiscoveryConvention this convention would still take precedence over the Id discovery convention since it is run first, but in the case where no “key” property is found, the “id” convention will run.</span></span>  <span data-ttu-id="e3125-142">Wir sehen dieses Verhalten, da jede Konvention des Modells angezeigt, wie in der vorherigen Konvention (statt sie unabhängig voneinander arbeiten und alle miteinander kombiniert werden) aktualisiert, sodass Wenn z. B. eine früheren Konvention den Namen einer Spalte nicht entsprechend aktualisiert interessante Ihrer benutzerdefinierten Konvention (wenn zuvor der Name nicht von Interesse war), und es gelten für diese Spalte.</span><span class="sxs-lookup"><span data-stu-id="e3125-142">We see this behavior because each convention sees the model as updated by the previous convention (rather than operating on it independently and all being combined together) so that if for example, a previous convention updated a column name to match something of interest to your custom convention (when before that the name was not of interest) then it will apply to that column.</span></span>  

``` csharp
public class BlogContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Comment> Comments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new CustomKeyDiscoveryConvention());
        modelBuilder.Conventions.Remove<IdKeyDiscoveryConvention>();
    }
}
```  

## <a name="notes"></a><span data-ttu-id="e3125-143">Hinweise</span><span class="sxs-lookup"><span data-stu-id="e3125-143">Notes</span></span>  

<span data-ttu-id="e3125-144">Eine Liste von Konventionen, die derzeit vom Entity Framework angewendet werden, ist verfügbar, in der MSDN-Dokumentation: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="e3125-144">A list of conventions that are currently applied by Entity Framework is available in the MSDN documentation here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  <span data-ttu-id="e3125-145">Diese Liste direkt über Ihren Quellcode per Pull abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="e3125-145">This list is pulled directly from our source code.</span></span>  <span data-ttu-id="e3125-146">Der Quellcode für Entity Framework 6 ist verfügbar auf [GitHub](https://github.com/aspnet/entityframework6/) und viele der Konventionen von Entity Framework verwendete sind gute Ausgangspunkte für benutzerdefinierte Modell Konventionen basierten.</span><span class="sxs-lookup"><span data-stu-id="e3125-146">The source code for Entity Framework 6 is available on [GitHub](https://github.com/aspnet/entityframework6/) and many of the conventions used by Entity Framework are good starting points for custom model based conventions.</span></span>  
