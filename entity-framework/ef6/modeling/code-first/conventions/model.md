---
title: Modellbasierten Konventionen - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: fb79164f71cb3afff705a83f5078a13d043abca8
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490934"
---
# <a name="model-based-conventions"></a>Modellbasierten Konventionen
> [!NOTE]
> **Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt. Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.  

Modellbasierter Konventionen sind eine erweiterte Konfiguration für das konventionsbasierten Methode. In den meisten Szenarien die [benutzerdefinierte Code First-Konvention-API auf DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) verwendet werden soll. Ein Überblick über den DbModelBuilder-API für Konventionen wird empfohlen, vor der Verwendung von Konventionen von Anwendungsmodellen basiert.  

Konventionen von Anwendungsmodellen basierend ermöglichen die Erstellung von Konventionen, die Auswirkungen auf die Eigenschaften und Tabellen, die nicht über die Standardkonventionen konfigurierbar sind. Beispiele hierfür sind Diskriminator in Tabelle pro Hierarchie Modelle und unabhängige Zuordnung Spalten.  

## <a name="creating-a-convention"></a>Erstellen eine Konvention   

Wenn in der Pipeline die Konvention für das Modell angewendet werden muss, ist der erste Schritt beim Erstellen einer Konvention basierendes Modell auswählen. Es gibt zwei Arten von Konventionen von Anwendungsmodellen, Konzept-(C-Bereich) und Store (S-Bereich) aus. Eine C-Space-Konvention wird für das Modell angewendet, die die Anwendung erstellt wird, während eine S-Space-Konvention auf die Version des Modells angewendet wird, die die Datenbank darstellt und Steuerelemente, beispielsweise wie automatisch generierten Spalten heißen.  

Eine modellkonvention ist eine Klasse, die von IConceptualModelConvention oder IStoreModelConvention erweitert.  Diese Schnittstellen, die beide einen generischen Typ akzeptieren, die von können geben MetadataItem die verwendet wird, um den Datentyp zu filtern, dem für die Konvention gilt.  

## <a name="adding-a-convention"></a>Hinzufügen einer Konvention   

Konventionen von Anwendungsmodellen werden in die gleiche Weise wie reguläre Konventionen Klassen hinzugefügt. In der **"onmodelcreating"** -Methode, die Konvention der Liste mit den Konventionen für ein Modell hinzufügen.  

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

Eine Konvention kann auch hinzugefügt werden, in Bezug auf eine andere Konvention, die mithilfe der Conventions.AddBefore\< \> oder Conventions.AddAfter\< \> Methoden. Weitere Informationen zu den Konventionen, die Entity Framework gilt finden Sie im Abschnitt Hinweise.  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a>Beispiel: Diskriminator-Modellkonvention  

Umbenennen von Spalten, die von EF generierten ist ein Beispiel von etwas, das mit der andere für APIs nicht möglich.  Dies ist eine Situation mit Konventionen von Anwendungsmodellen Ihre einzige Option ist.  

Verdeutlicht, wie Sie eine Konvention basierendes Modell verwenden, um generierten Spalten zu konfigurieren ist die Methode anpassen, die als Unterscheidungsspalte Spalten identisch sind.  Im folgenden finden Sie ein Beispiel für eine einfache Modellbasierter-Konvention, die jede Spalte im Modell mit dem Namen "Discriminator" an "EntityType" umbenannt.  Dies schließt Spalten an, dass der Entwickler einfach mit der Bezeichnung "Discriminator genannt". Da die Spalte "Discriminator" genannt. eine generierte Spalte ist muss dies im S-Bereich ausgeführt.  

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

## <a name="example-general-ia-renaming-convention"></a>Beispiel: Allgemeine IA-Konvention umbenennen   

Ein weiteres etwas komplizierteres Beispiel basierendes modellkonventionen in Aktion ist können Sie konfigurieren, dass unabhängige Zuordnungen (IAs) benannt werden.  Dies ist eine Situation, in denen Konventionen von Anwendungsmodellen anwendbar sind, da IAs von EF generiert werden, und sind nicht vorhanden ist, im Modell, das von der DbModelBuilder-API zugreifen können.  

Wenn EF eine IA generiert, wird eine Spalte namens EntityType_KeyName. Z. B. mit dem Namen für eine Zuordnung mit dem Namen Customer, mit einer Schlüsselspalte "CustomerID", die sie eine Spalte namens Customer_CustomerId generieren würden. Die folgende Konvention Streifen der "\_'-Zeichen aus den Namen der Spalte, die für den IA generiert wird  

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

## <a name="extending-existing-conventions"></a>Erweitern Sie vorhandene Konventionen   

Falls also eine Konvention zu schreiben, die einem der Konventionen entspricht, die Entity Framework bereits auf das Modell angewendet wird, können Sie immer diese Konvention zur Vermeidung von Grund auf neu schreiben müssen, erweitern.  Ein Beispiel hierfür ist, Ersetzen der vorhandene Id, die, die Konvention, die durch eine benutzerdefinierte.   Ein weiterer Vorteil ist, überschreiben die wichtigsten Konvention ist, dass die überschriebene Methode aufgerufen wird nur dann, wenn kein Schlüssel bereits erkannt oder explizit konfiguriert. Eine Liste von Konventionen, die von Entity Framework verwendet ist hier verfügbar: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  

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

Dann müssen wir unsere neue Konvention vor der vorhandenen Schlüssel Konvention hinzufügen. Nachdem wir die CustomKeyDiscoveryConvention hinzugefügt haben, können wir die IdKeyDiscoveryConvention entfernen.  Wenn wir die vorhandenen IdKeyDiscoveryConvention entfernt haben, dieser Konvention noch Vorrang vor würden die Id-Discovery-Konvention, da er ausgeführt wird, zuerst, aber in die Groß-/Kleinschreibung, keine "Key"-Eigenschaft gefunden wird, wird die Konvention "Id" ausgeführt.  Wir sehen dieses Verhalten, da jede Konvention des Modells angezeigt, wie in der vorherigen Konvention (statt sie unabhängig voneinander arbeiten und alle miteinander kombiniert werden) aktualisiert, sodass Wenn z. B. eine früheren Konvention den Namen einer Spalte nicht entsprechend aktualisiert interessante Ihrer benutzerdefinierten Konvention (wenn zuvor der Name nicht von Interesse war), und es gelten für diese Spalte.  

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

## <a name="notes"></a>Hinweise  

Eine Liste von Konventionen, die derzeit vom Entity Framework angewendet werden, ist verfügbar, in der MSDN-Dokumentation: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  Diese Liste direkt über Ihren Quellcode per Pull abgerufen wird.  Der Quellcode für Entity Framework 6 ist verfügbar auf [GitHub](https://github.com/aspnet/entityframework6/) und viele der Konventionen von Entity Framework verwendete sind gute Ausgangspunkte für benutzerdefinierte Modell Konventionen basierten.  
