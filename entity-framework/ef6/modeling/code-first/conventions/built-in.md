---
title: Code First Konventionen-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
ms.openlocfilehash: 4d03a32db5d84eb37c22617a95005b272172a65d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415946"
---
# <a name="code-first-conventions"></a><span data-ttu-id="33d88-102">Code First Konventionen</span><span class="sxs-lookup"><span data-stu-id="33d88-102">Code First Conventions</span></span>
<span data-ttu-id="33d88-103">Code First ermöglicht es Ihnen, ein Modell mithilfe C# von oder Visual Basic .NET-Klassen zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="33d88-103">Code First enables you to describe a model by using C# or Visual Basic .NET classes.</span></span> <span data-ttu-id="33d88-104">Die grundlegende Form des Modells wird mithilfe von Konventionen erkannt.</span><span class="sxs-lookup"><span data-stu-id="33d88-104">The basic shape of the model is detected by using conventions.</span></span> <span data-ttu-id="33d88-105">Konventionen sind Sätze von Regeln, die verwendet werden, um ein konzeptionelles Modell, das auf Klassendefinitionen basiert, bei der Arbeit mit Code First automatisch zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="33d88-105">Conventions are sets of rules that are used to automatically configure a conceptual model based on class definitions when working with Code First.</span></span> <span data-ttu-id="33d88-106">Die Konventionen werden im Namespace System. Data. Entity. modelconfiguration. Conventions definiert.</span><span class="sxs-lookup"><span data-stu-id="33d88-106">The conventions are defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>  

<span data-ttu-id="33d88-107">Sie können Ihr Modell mithilfe von Daten Anmerkungen oder der fließenden API weiter konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="33d88-107">You can further configure your model by using data annotations or the fluent API.</span></span> <span data-ttu-id="33d88-108">Die-Konfiguration wird durch die fließende API, gefolgt von Daten Anmerkungen und then-Konventionen, Vorrang eingeräumt.</span><span class="sxs-lookup"><span data-stu-id="33d88-108">Precedence is given to configuration through the fluent API followed by data annotations and then conventions.</span></span> <span data-ttu-id="33d88-109">Weitere Informationen finden Sie unter [Daten Anmerkungen](~/ef6/modeling/code-first/data-annotations.md), [fließende API-Beziehungen](~/ef6/modeling/code-first/fluent/relationships.md), [fließende API-Typen & Eigenschaften](~/ef6/modeling/code-first/fluent/types-and-properties.md) und eine [fließende API mit VB.net](~/ef6/modeling/code-first/fluent/vb.md).</span><span class="sxs-lookup"><span data-stu-id="33d88-109">For more information see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md), [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API - Types & Properties](~/ef6/modeling/code-first/fluent/types-and-properties.md) and [Fluent API with VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span></span>  

<span data-ttu-id="33d88-110">Eine ausführliche Liste der Code First Konventionen finden Sie in der [API-Dokumentation](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="33d88-110">A detailed list of Code First conventions is available in the [API Documentation](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span> <span data-ttu-id="33d88-111">Dieses Thema enthält eine Übersicht über die Konventionen, die von Code First verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="33d88-111">This topic provides an overview of the conventions used by Code First.</span></span>  

## <a name="type-discovery"></a><span data-ttu-id="33d88-112">Typanmittlung</span><span class="sxs-lookup"><span data-stu-id="33d88-112">Type Discovery</span></span>  

<span data-ttu-id="33d88-113">Wenn Sie Code First Entwicklung verwenden, schreiben Sie in der Regel .NET Framework Klassen, die ihr konzeptionelles (Domänen-) Modell definieren.</span><span class="sxs-lookup"><span data-stu-id="33d88-113">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="33d88-114">Zusätzlich zum Definieren der Klassen müssen Sie **dbcontext** auch darüber informieren, welche Typen Sie in das Modell einschließen möchten.</span><span class="sxs-lookup"><span data-stu-id="33d88-114">In addition to defining the classes, you also need to let **DbContext** know which types you want to include in the model.</span></span> <span data-ttu-id="33d88-115">Zu diesem Zweck definieren Sie eine Kontext Klasse, die von **dbcontext** abgeleitet ist und für die Typen, die Sie als Teil des Modells verwenden möchten, **dbset** -Eigenschaften verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="33d88-115">To do this, you define a context class that derives from **DbContext** and exposes **DbSet** properties for the types that you want to be part of the model.</span></span> <span data-ttu-id="33d88-116">Code First diese Typen einschließen und auch alle referenzierten Typen abrufen, auch wenn die Typen, auf die verwiesen wird, in einer anderen Assembly definiert sind.</span><span class="sxs-lookup"><span data-stu-id="33d88-116">Code First will include these types and also will pull in any referenced types, even if the referenced types are defined in a different assembly.</span></span>  

<span data-ttu-id="33d88-117">Wenn Ihre Typen an einer Vererbungs Hierarchie teilnehmen, genügt es, eine **dbset** -Eigenschaft für die Basisklasse zu definieren, und die abgeleiteten Typen werden automatisch eingeschlossen, wenn Sie sich in derselben Assembly wie die Basisklasse befinden.</span><span class="sxs-lookup"><span data-stu-id="33d88-117">If your types participate in an inheritance hierarchy, it is enough to define a **DbSet** property for the base class, and the derived types will be automatically included, if they are in the same assembly as the base class.</span></span>  

<span data-ttu-id="33d88-118">Im folgenden Beispiel wird nur eine **dbset** -Eigenschaft für die **SchoolEntities** -Klasse (**Abteilungen**) definiert.</span><span class="sxs-lookup"><span data-stu-id="33d88-118">In the following example, there is only one **DbSet** property defined on the **SchoolEntities** class (**Departments**).</span></span> <span data-ttu-id="33d88-119">Code First verwendet diese Eigenschaft zum Ermitteln und Abrufen aller referenzierten Typen.</span><span class="sxs-lookup"><span data-stu-id="33d88-119">Code First uses this property to discover and pull in any referenced types.</span></span>  

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

<span data-ttu-id="33d88-120">Wenn Sie einen Typ aus dem Modell ausschließen möchten, verwenden Sie das Attribut " **notmapping** " oder die " **dbmodelbuilder. Ignore** "-API.</span><span class="sxs-lookup"><span data-stu-id="33d88-120">If you want to exclude a type from the model, use the **NotMapped** attribute or the **DbModelBuilder.Ignore** fluent API.</span></span>  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a><span data-ttu-id="33d88-121">Primärschlüssel Konvention</span><span class="sxs-lookup"><span data-stu-id="33d88-121">Primary Key Convention</span></span>  

<span data-ttu-id="33d88-122">Code First ab, dass eine Eigenschaft ein Primärschlüssel ist, wenn eine Eigenschaft in einer Klasse mit dem Namen "ID" (ohne Beachtung der Groß-/Kleinschreibung) oder dem Klassennamen gefolgt von "ID" benannt wird.</span><span class="sxs-lookup"><span data-stu-id="33d88-122">Code First infers that a property is a primary key if a property on a class is named “ID” (not case sensitive), or the class name followed by "ID".</span></span> <span data-ttu-id="33d88-123">Wenn der Typ der Primärschlüssel Eigenschaft numerisch oder GUID ist, wird er als Identitäts Spalte konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="33d88-123">If the type of the primary key property is numeric or GUID it will be configured as an identity column.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a><span data-ttu-id="33d88-124">Beziehungs Konvention</span><span class="sxs-lookup"><span data-stu-id="33d88-124">Relationship Convention</span></span>  

<span data-ttu-id="33d88-125">In Entity Framework bieten Navigations Eigenschaften eine Möglichkeit, eine Beziehung zwischen zwei Entitäts Typen zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="33d88-125">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span> <span data-ttu-id="33d88-126">Jedes Objekt kann für jede Beziehung, an der es beteiligt ist, über eine Navigationseigenschaft verfügen.</span><span class="sxs-lookup"><span data-stu-id="33d88-126">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="33d88-127">Mithilfe von Navigations Eigenschaften können Sie Beziehungen in beiden Richtungen navigieren und verwalten, wobei entweder ein Verweis Objekt (wenn die Multiplizität entweder 1 oder 0 ist) oder eine Auflistung (wenn die Multiplizität viele ist) zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="33d88-127">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="33d88-128">Code First leitet Beziehungen auf der Grundlage der Navigations Eigenschaften ab, die für Ihre Typen definiert sind.</span><span class="sxs-lookup"><span data-stu-id="33d88-128">Code First infers relationships based on the navigation properties defined on your types.</span></span>  

<span data-ttu-id="33d88-129">Zusätzlich zu den Navigations Eigenschaften empfiehlt es sich, Fremdschlüssel Eigenschaften für die Typen einzuschließen, die abhängige Objekte darstellen.</span><span class="sxs-lookup"><span data-stu-id="33d88-129">In addition to navigation properties, we recommend that you include foreign key properties on the types that represent dependent objects.</span></span> <span data-ttu-id="33d88-130">Jede Eigenschaft mit demselben Datentyp wie die Prinzipal-Primärschlüssel Eigenschaft und mit einem Namen, der einem der folgenden Formate folgt, stellt einen Fremdschlüssel für die Beziehung dar: "\<Navigations Eigenschaftsname\>\<Prinzipal Name der Primärschlüssel Eigenschaft\>", "\<Prinzipal Klassenname\>\<Name der Primärschlüssel Eigenschaft\>" oder "\<Prinzipal Name der Primärschlüssel Eigenschaft\></span><span class="sxs-lookup"><span data-stu-id="33d88-130">Any property with the same data type as the principal primary key property and with a name that follows one of the following formats represents a foreign key for the relationship: '\<navigation property name\>\<principal primary key property name\>', '\<principal class name\>\<primary key property name\>', or '\<principal primary key property name\>'.</span></span> <span data-ttu-id="33d88-131">Wenn mehrere Übereinstimmungen gefunden werden, wird in der oben aufgeführten Reihenfolge Vorrang eingeräumt.</span><span class="sxs-lookup"><span data-stu-id="33d88-131">If multiple matches are found then precedence is given in the order listed above.</span></span> <span data-ttu-id="33d88-132">Bei der Fremdschlüssel Erkennung wird die Groß-/Kleinschreibung</span><span class="sxs-lookup"><span data-stu-id="33d88-132">Foreign key detection is not case sensitive.</span></span> <span data-ttu-id="33d88-133">Wenn eine Fremdschlüssel Eigenschaft erkannt wird, leitet Code First die Multiplizität der Beziehung basierend auf der NULL-Zulässigkeit des fremd Schlüssels ab.</span><span class="sxs-lookup"><span data-stu-id="33d88-133">When a foreign key property is detected, Code First infers the multiplicity of the relationship based on the nullability of the foreign key.</span></span> <span data-ttu-id="33d88-134">Wenn die Eigenschaft NULL-Werte zulässt, wird die Beziehung als optional registriert. Andernfalls wird die Beziehung nach Bedarf registriert.</span><span class="sxs-lookup"><span data-stu-id="33d88-134">If the property is nullable then the relationship is registered as optional; otherwise the relationship is registered as required.</span></span>  

<span data-ttu-id="33d88-135">Wenn ein Fremdschlüssel für die abhängige Entität keine NULL-Werte zulässt, legt Code First den Löschvorgang für die Beziehung fest.</span><span class="sxs-lookup"><span data-stu-id="33d88-135">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="33d88-136">Wenn ein Fremdschlüssel für die abhängige Entität NULL-Werte zulässt, legt Code First für die Beziehung keine Lösch Weitergabe fest, und wenn der Prinzipal gelöscht wird, wird der Fremdschlüssel auf NULL festgelegt.</span><span class="sxs-lookup"><span data-stu-id="33d88-136">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span> <span data-ttu-id="33d88-137">Das von der Konvention erkannte Multiplizität und Lösch Verhalten können überschrieben werden, indem die fließende API verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="33d88-137">The multiplicity and cascade delete behavior detected by convention can be overridden by using the fluent API.</span></span>  

<span data-ttu-id="33d88-138">Im folgenden Beispiel werden die Navigations Eigenschaften und ein Fremdschlüssel verwendet, um die Beziehung zwischen den Klassen "Department" und "Course" zu definieren.</span><span class="sxs-lookup"><span data-stu-id="33d88-138">In the following example the navigation properties and a foreign key are used to define the relationship between the Department and Course classes.</span></span>  

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
> <span data-ttu-id="33d88-139">Wenn Sie über mehrere Beziehungen zwischen denselben Typen verfügen (z. b. angenommen, Sie definieren die **Personen** -und **Buch** Klassen, in denen die **Person** -Klasse die Navigations Eigenschaften **reviewedbooks** und **authoredbooks** enthält und die **Book** -Klasse die Navigations Eigenschaften **Author** und **Reviewer** enthält), müssen Sie die Beziehungen mithilfe von Daten Anmerkungen oder der fließenden API manuell konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="33d88-139">If you have multiple relationships between the same types (for example, suppose you define the **Person** and **Book** classes, where the **Person** class contains the **ReviewedBooks** and **AuthoredBooks** navigation properties and the **Book** class contains the **Author** and **Reviewer** navigation properties) you need to manually configure the relationships by using Data Annotations or the fluent API.</span></span> <span data-ttu-id="33d88-140">Weitere Informationen finden Sie unter [Daten Anmerkungen-Beziehungen](~/ef6/modeling/code-first/data-annotations.md) und [fließende API-Beziehungen](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="33d88-140">For more information, see [Data Annotations - Relationships](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>  

## <a name="complex-types-convention"></a><span data-ttu-id="33d88-141">Komplexe Typen Konvention</span><span class="sxs-lookup"><span data-stu-id="33d88-141">Complex Types Convention</span></span>  

<span data-ttu-id="33d88-142">Wenn Code First eine Klassendefinition ermittelt, in der ein Primärschlüssel nicht abgeleitet werden kann, und kein Primärschlüssel durch Daten Anmerkungen oder die fließende API registriert ist, wird der Typ automatisch als komplexer Typ registriert.</span><span class="sxs-lookup"><span data-stu-id="33d88-142">When Code First discovers a class definition where a primary key cannot be inferred, and no primary key is registered through data annotations or the fluent API, then the type is automatically registered as a complex type.</span></span> <span data-ttu-id="33d88-143">Die Erkennung komplexer Typen erfordert auch, dass der Typ keine Eigenschaften hat, die auf Entitäts Typen verweisen, und auf die von einer Auflistungs Eigenschaft eines anderen Typs nicht verwiesen wird.</span><span class="sxs-lookup"><span data-stu-id="33d88-143">Complex type detection also requires that the type does not have properties that reference entity types and is not referenced from a collection property on another type.</span></span> <span data-ttu-id="33d88-144">Wenn die folgenden Klassendefinitionen angegeben werden, wird Code First abgeleitet, dass die **Details** ein komplexer Typ ist, da er keinen Primärschlüssel aufweist.</span><span class="sxs-lookup"><span data-stu-id="33d88-144">Given the following class definitions Code First would infer that **Details** is a complex type because it has no primary key.</span></span>  

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

## <a name="connection-string-convention"></a><span data-ttu-id="33d88-145">Verbindungs Zeichen folgen Konvention</span><span class="sxs-lookup"><span data-stu-id="33d88-145">Connection String Convention</span></span>  

<span data-ttu-id="33d88-146">Informationen zu den Konventionen, die dbcontext zum Ermitteln der zu verwendenden Verbindung verwendet, finden Sie unter [Verbindungen und Modelle](~/ef6/fundamentals/configuring/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="33d88-146">To learn about the conventions that DbContext uses to discover the connection to use see [Connections and Models](~/ef6/fundamentals/configuring/connection-strings.md).</span></span>  

## <a name="removing-conventions"></a><span data-ttu-id="33d88-147">Entfernen von Konventionen</span><span class="sxs-lookup"><span data-stu-id="33d88-147">Removing Conventions</span></span>  

<span data-ttu-id="33d88-148">Sie können alle Konventionen entfernen, die im System. Data. Entity. modelconfiguration. Conventions-Namespace definiert sind.</span><span class="sxs-lookup"><span data-stu-id="33d88-148">You can remove any of the conventions defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span> <span data-ttu-id="33d88-149">Im folgenden Beispiel wird **pluralizingtablenameconvention**entfernt.</span><span class="sxs-lookup"><span data-stu-id="33d88-149">The following example removes **PluralizingTableNameConvention**.</span></span>  

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

## <a name="custom-conventions"></a><span data-ttu-id="33d88-150">Benutzerdefinierte Konventionen</span><span class="sxs-lookup"><span data-stu-id="33d88-150">Custom Conventions</span></span>  

<span data-ttu-id="33d88-151">Benutzerdefinierte Konventionen werden ab EF6 unterstützt.</span><span class="sxs-lookup"><span data-stu-id="33d88-151">Custom conventions are supported in EF6 onwards.</span></span> <span data-ttu-id="33d88-152">Weitere Informationen finden Sie unter [benutzerdefinierte Code First Konventionen](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="33d88-152">For more information see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>
