---
title: Code First-Konventionen - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
caps.latest.revision: 4
ms.openlocfilehash: b124a034ba900780cc4d7e1354408cd3995e874e
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2018
ms.locfileid: "39120814"
---
# <a name="code-first-conventions"></a><span data-ttu-id="112b7-102">Code First-Konventionen</span><span class="sxs-lookup"><span data-stu-id="112b7-102">Code First Conventions</span></span>
<span data-ttu-id="112b7-103">Code First können Sie zum Beschreiben eines Modells mithilfe von c# oder Visual Basic-Klassen.</span><span class="sxs-lookup"><span data-stu-id="112b7-103">Code First enables you to describe a model by using C# or Visual Basic .NET classes.</span></span> <span data-ttu-id="112b7-104">Die grundlegende Form des Modells wird mithilfe von Konventionen erkannt.</span><span class="sxs-lookup"><span data-stu-id="112b7-104">The basic shape of the model is detected by using conventions.</span></span> <span data-ttu-id="112b7-105">Konventionen sind Sätze von Regeln, die verwendet werden, um ein konzeptionelles Modell basierend auf Klassendefinitionen, bei der Arbeit mit Code First automatisch zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="112b7-105">Conventions are sets of rules that are used to automatically configure a conceptual model based on class definitions when working with Code First.</span></span> <span data-ttu-id="112b7-106">Die Konventionen werden in den System.Data.Entity.ModelConfiguration.Conventions-Namespace definiert.</span><span class="sxs-lookup"><span data-stu-id="112b7-106">The conventions are defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>  

<span data-ttu-id="112b7-107">Sie können das Modell weiter mithilfe von datenanmerkungen oder der fluent-API konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="112b7-107">You can further configure your model by using data annotations or the fluent API.</span></span> <span data-ttu-id="112b7-108">Die Konfiguration über die fluent-API, gefolgt von datenanmerkungen und Konventionen erhält Vorrang vor.</span><span class="sxs-lookup"><span data-stu-id="112b7-108">Precedence is given to configuration through the fluent API followed by data annotations and then conventions.</span></span> <span data-ttu-id="112b7-109">Weitere Informationen finden Sie unter [Datenanmerkungen](~/ef6/modeling/code-first/data-annotations.md), [Fluent-API – Beziehungen](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent-API - Typen und Eigenschaften, die](~/ef6/modeling/code-first/fluent/types-and-properties.md) und [Fluent-API in VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span><span class="sxs-lookup"><span data-stu-id="112b7-109">For more information see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md), [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API - Types & Properties](~/ef6/modeling/code-first/fluent/types-and-properties.md) and [Fluent API with VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span></span>  

<span data-ttu-id="112b7-110">Eine detaillierte Liste mit Code First-Konventionen finden Sie in der [-API-Dokumentation](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="112b7-110">A detailed list of Code First conventions is available in the [API Documentation](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span> <span data-ttu-id="112b7-111">Dieses Thema enthält eine Übersicht über die Konventionen, die von Code First verwendet.</span><span class="sxs-lookup"><span data-stu-id="112b7-111">This topic provides an overview of the conventions used by Code First.</span></span>  

## <a name="type-discovery"></a><span data-ttu-id="112b7-112">Typermittlung</span><span class="sxs-lookup"><span data-stu-id="112b7-112">Type Discovery</span></span>  

<span data-ttu-id="112b7-113">Bei Verwendung von Code First-Entwicklung beginnen Sie in der Regel durch das Schreiben von .NET Framework-Klassen, die das konzeptionelle (Domäne)-Modell zu definieren.</span><span class="sxs-lookup"><span data-stu-id="112b7-113">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="112b7-114">Zusätzlich zum Definieren von Klassen, müssen Sie auch können **"DbContext"** wissen, welche Typen im Modell enthalten sein sollen.</span><span class="sxs-lookup"><span data-stu-id="112b7-114">In addition to defining the classes, you also need to let **DbContext** know which types you want to include in the model.</span></span> <span data-ttu-id="112b7-115">Zu diesem Zweck definieren Sie eine Kontextklasse, die von abgeleitet **"DbContext"** und macht **"DbSet"** Eigenschaften für die Typen, die Teil des Modells sein sollen.</span><span class="sxs-lookup"><span data-stu-id="112b7-115">To do this, you define a context class that derives from **DbContext** and exposes **DbSet** properties for the types that you want to be part of the model.</span></span> <span data-ttu-id="112b7-116">Code zuerst diese Typen enthalten, und auch wird per Pullvorgang in Typen auf die verwiesen wird, auch wenn der referenzierten Typen in einer anderen Assembly definiert sind.</span><span class="sxs-lookup"><span data-stu-id="112b7-116">Code First will include these types and also will pull in any referenced types, even if the referenced types are defined in a different assembly.</span></span>  

<span data-ttu-id="112b7-117">Wenn die Typen in einer Vererbungshierarchie beteiligt, es ist ausreichend, definieren Sie eine **"DbSet"** -Eigenschaft für die Basisklasse und abgeleiteten Typen werden automatisch eingeschlossen, wenn sie in der gleichen Assembly wie der Basisklasse sind.</span><span class="sxs-lookup"><span data-stu-id="112b7-117">If your types participate in an inheritance hierarchy, it is enough to define a **DbSet** property for the base class, and the derived types will be automatically included, if they are in the same assembly as the base class.</span></span>  

<span data-ttu-id="112b7-118">Im folgenden Beispiel ist nur ein **"DbSet"** -Eigenschaft definiert, auf die **SchoolEntities** Klasse (**Abteilungen**).</span><span class="sxs-lookup"><span data-stu-id="112b7-118">In the following example, there is only one **DbSet** property defined on the **SchoolEntities** class (**Departments**).</span></span> <span data-ttu-id="112b7-119">Code wird diese Eigenschaft zunächst ermitteln und alle referenzierten Typen an. beziehen.</span><span class="sxs-lookup"><span data-stu-id="112b7-119">Code First uses this property to discover and pull in any referenced types.</span></span>  

``` csharp
public class SchoolEntities : DbContext
{
    public DbSet<Department> Departments { get; set; }
}

public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

public partial class OnsiteCourse : Course
{
    public string Location { get; set; }
    public string Days { get; set; }
    public System.DateTime Time { get; set; }
}
```  

<span data-ttu-id="112b7-120">Wenn Sie einen Typ aus dem Modell ausschließen möchten, verwenden Sie die **NotMapped** Attribut oder das **DbModelBuilder.Ignore** fluent-API.</span><span class="sxs-lookup"><span data-stu-id="112b7-120">If you want to exclude a type from the model, use the **NotMapped** attribute or the **DbModelBuilder.Ignore** fluent API.</span></span>  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a><span data-ttu-id="112b7-121">Primary Key-Konvention</span><span class="sxs-lookup"><span data-stu-id="112b7-121">Primary Key Convention</span></span>  

<span data-ttu-id="112b7-122">Code leitet zunächst, dass eine Eigenschaft ein primärer Schlüssel ist, wenn eine Eigenschaft einer Klasse "ID" (ohne Beachtung von Groß-/Kleinschreibung heißt) oder der Klassennamen gefolgt von "ID".</span><span class="sxs-lookup"><span data-stu-id="112b7-122">Code First infers that a property is a primary key if a property on a class is named “ID” (not case sensitive), or the class name followed by "ID".</span></span> <span data-ttu-id="112b7-123">Wenn der Typ der Eigenschaft des primären Schlüssels numerisch ist oder GUID werden als eine Identity-Spalte konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="112b7-123">If the type of the primary key property is numeric or GUID it will be configured as an identity column.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a><span data-ttu-id="112b7-124">Beziehung Konvention</span><span class="sxs-lookup"><span data-stu-id="112b7-124">Relationship Convention</span></span>  

<span data-ttu-id="112b7-125">Entity Framework bieten Ihnen eine Möglichkeit, eine Beziehung zwischen zwei Entitätstypen zu navigieren Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="112b7-125">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span> <span data-ttu-id="112b7-126">Jedes Objekt kann für jede Beziehung, an der es beteiligt ist, über eine Navigationseigenschaft verfügen.</span><span class="sxs-lookup"><span data-stu-id="112b7-126">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="112b7-127">Navigationseigenschaften können Sie zum Navigieren und Verwalten von Beziehungen in beide Richtungen, entweder ein Verweisobjekt zurückgegeben (wenn die Multiplizität entweder nacheinander oder 0 (null) oder 1) oder eine Sammlung (wenn die Multiplizität n ist).</span><span class="sxs-lookup"><span data-stu-id="112b7-127">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="112b7-128">Zuerst leitet Code Beziehungen basierend auf die in Ihre Typen definierten Navigationseigenschaften ab.</span><span class="sxs-lookup"><span data-stu-id="112b7-128">Code First infers relationships based on the navigation properties defined on your types.</span></span>  

<span data-ttu-id="112b7-129">Zusätzlich zu Navigationseigenschaften empfehlen wir, dass Sie Fremdschlüsseleigenschaften für die Typen enthalten, die abhängigen Objekte darstellen.</span><span class="sxs-lookup"><span data-stu-id="112b7-129">In addition to navigation properties, we recommend that you include foreign key properties on the types that represent dependent objects.</span></span> <span data-ttu-id="112b7-130">Eine Eigenschaft mit dem gleichen Datentyp wie der Dienstprinzipal Primärschlüsseleigenschaft und mit einem Namen, der auf einem der folgenden Formate folgt, stellt einen Fremdschlüssel für die Beziehung: "\<navigationseigenschaftennamen\>\<Dienstprinzipal Primärschlüsseleigenschaft Namen\>','\<Prinzipalklasse Namen\>\<Primärschlüsseleigenschaft Namen\>", oder"\<principal Primärschlüsseleigenschaft Namen\>".</span><span class="sxs-lookup"><span data-stu-id="112b7-130">Any property with the same data type as the principal primary key property and with a name that follows one of the following formats represents a foreign key for the relationship: '\<navigation property name\>\<principal primary key property name\>', '\<principal class name\>\<primary key property name\>', or '\<principal primary key property name\>'.</span></span> <span data-ttu-id="112b7-131">Wenn mehrere Übereinstimmungen gefunden werden, und klicken Sie dann Vorrang vor in der obigen Reihenfolge angegeben ist.</span><span class="sxs-lookup"><span data-stu-id="112b7-131">If multiple matches are found then precedence is given in the order listed above.</span></span> <span data-ttu-id="112b7-132">Foreign Key-Erkennung ist nicht in der Groß-/Kleinschreibung beachtet.</span><span class="sxs-lookup"><span data-stu-id="112b7-132">Foreign key detection is not case sensitive.</span></span> <span data-ttu-id="112b7-133">Wenn eine Fremdschlüsseleigenschaft erkannt wird, leitet Code First die Multiplizität der Beziehung basierend auf der NULL-Zulässigkeit des Fremdschlüssels.</span><span class="sxs-lookup"><span data-stu-id="112b7-133">When a foreign key property is detected, Code First infers the multiplicity of the relationship based on the nullability of the foreign key.</span></span> <span data-ttu-id="112b7-134">Wenn die Eigenschaft auf NULL festlegbar ist. ist die Beziehung als optional registrierten; Andernfalls wird die Beziehung registriert nach Bedarf.</span><span class="sxs-lookup"><span data-stu-id="112b7-134">If the property is nullable then the relationship is registered as optional; otherwise the relationship is registered as required.</span></span>  

<span data-ttu-id="112b7-135">Ist ein Fremdschlüssel für die abhängige Entität nicht NULL-Werte zulässt, legt dann Code First Löschweitergabe für die Beziehung.</span><span class="sxs-lookup"><span data-stu-id="112b7-135">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="112b7-136">Wenn ein Fremdschlüssel in der abhängigen Entität NULL-Werte zulässt ist, Code First Löschweitergabe nicht auf der Beziehung fest, und wenn der Prinzipal gelöscht wird die foreign Key wird festgelegt auf Null.</span><span class="sxs-lookup"><span data-stu-id="112b7-136">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span> <span data-ttu-id="112b7-137">Löschen Sie die Multiplizität und Cascade Verhalten von erkannten Konvention kann mithilfe der fluent-API überschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="112b7-137">The multiplicity and cascade delete behavior detected by convention can be overridden by using the fluent API.</span></span>  

<span data-ttu-id="112b7-138">Im folgenden Beispiel werden die Navigationseigenschaften und ein Fremdschlüssel verwendet, zum Definieren der Beziehung zwischen den Klassen-Abteilung und Kurs.</span><span class="sxs-lookup"><span data-stu-id="112b7-138">In the following example the navigation properties and a foreign key are used to define the relationship between the Department and Course classes.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}
```  

> [!NOTE]
> <span data-ttu-id="112b7-139">Wenn Sie über mehrere Beziehungen zwischen den gleichen Typen verfügen (Nehmen wir beispielsweise an, die Sie definieren die **Person** und **Buch** Klassen, in denen die **Person** -Klasse enthält die  **ReviewedBooks** und **AuthoredBooks** Navigationseigenschaften und **Buch** -Klasse enthält die **Autor** und  **Prüfer** Navigationseigenschaften) müssen Sie die Beziehungen manuell zu konfigurieren, mithilfe von Datenanmerkungen oder der fluent-API.</span><span class="sxs-lookup"><span data-stu-id="112b7-139">If you have multiple relationships between the same types (for example, suppose you define the **Person** and **Book** classes, where the **Person** class contains the **ReviewedBooks** and **AuthoredBooks** navigation properties and the **Book** class contains the **Author** and **Reviewer** navigation properties) you need to manually configure the relationships by using Data Annotations or the fluent API.</span></span> <span data-ttu-id="112b7-140">Weitere Informationen finden Sie unter [Datenanmerkungen - Beziehungen](~/ef6/modeling/code-first/data-annotations.md) und [Fluent-API – Beziehungen](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="112b7-140">For more information, see [Data Annotations - Relationships](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>  

## <a name="complex-types-convention"></a><span data-ttu-id="112b7-141">Konvention für komplexe Typen</span><span class="sxs-lookup"><span data-stu-id="112b7-141">Complex Types Convention</span></span>  

<span data-ttu-id="112b7-142">Wenn Code First eine Klassendefinition ermittelt, in dem ein primärer Schlüssel kann nicht abgeleitet werden, und keinen primärer Schlüssel mithilfe von datenanmerkungen oder der fluent-API registriert ist, wird der Typ als komplexen Typ automatisch registriert.</span><span class="sxs-lookup"><span data-stu-id="112b7-142">When Code First discovers a class definition where a primary key cannot be inferred, and no primary key is registered through data annotations or the fluent API, then the type is automatically registered as a complex type.</span></span> <span data-ttu-id="112b7-143">Erkennung von komplexen Typs erfordert auch, dass der Typ keine Eigenschaften besitzt, die Entitätstypen verweisen und nicht aus einer Auflistungseigenschaft auf einen anderen Typ verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="112b7-143">Complex type detection also requires that the type does not have properties that reference entity types and is not referenced from a collection property on another type.</span></span> <span data-ttu-id="112b7-144">Erhalten die folgenden Klassendefinitionen Code First würde abzuleiten, die **Details** ist ein komplexer Typ, da es keinen Primärschlüssel aufweist.</span><span class="sxs-lookup"><span data-stu-id="112b7-144">Given the following class definitions Code First would infer that **Details** is a complex type because it has no primary key.</span></span>  

``` csharp
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
```  

## <a name="connection-string-convention"></a><span data-ttu-id="112b7-145">Verbindung Zeichenfolgenkonventionen</span><span class="sxs-lookup"><span data-stu-id="112b7-145">Connection String Convention</span></span>  

<span data-ttu-id="112b7-146">Die Konventionen erfahren Sie, dass die Verbindung finden Sie unter ermittelt mithilfe von "DbContext" [Verbindungen und Modelle](~/ef6/fundamentals/configuring/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="112b7-146">To learn about the conventions that DbContext uses to discover the connection to use see [Connections and Models](~/ef6/fundamentals/configuring/connection-strings.md).</span></span>  

## <a name="removing-conventions"></a><span data-ttu-id="112b7-147">Entfernen von Konventionen</span><span class="sxs-lookup"><span data-stu-id="112b7-147">Removing Conventions</span></span>  

<span data-ttu-id="112b7-148">Sie können die Konventionen, die im Namespace System.Data.Entity.ModelConfiguration.Conventions definierten entfernen.</span><span class="sxs-lookup"><span data-stu-id="112b7-148">You can remove any of the conventions defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span> <span data-ttu-id="112b7-149">Im folgenden Beispiel wird **PluralizingTableNameConvention**.</span><span class="sxs-lookup"><span data-stu-id="112b7-149">The following example removes **PluralizingTableNameConvention**.</span></span>  

``` csharp
public class SchoolEntities : DbContext
{
     . . .

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention, the generated tables  
        // will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}
```  

## <a name="custom-conventions"></a><span data-ttu-id="112b7-150">Benutzerdefinierte Konventionen</span><span class="sxs-lookup"><span data-stu-id="112b7-150">Custom Conventions</span></span>  

<span data-ttu-id="112b7-151">Benutzerdefinierte Konventionen werden in EF6 oder höher unterstützt.</span><span class="sxs-lookup"><span data-stu-id="112b7-151">Custom conventions are supported in EF6 onwards.</span></span> <span data-ttu-id="112b7-152">Weitere Informationen finden Sie unter [benutzerdefinierte Code First-Konventionen](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="112b7-152">For more information see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>
