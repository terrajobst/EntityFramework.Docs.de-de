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
# <a name="code-first-conventions"></a>Code First-Konventionen
Code First können Sie zum Beschreiben eines Modells mithilfe von c# oder Visual Basic-Klassen. Die grundlegende Form des Modells wird mithilfe von Konventionen erkannt. Konventionen sind Sätze von Regeln, die verwendet werden, um ein konzeptionelles Modell basierend auf Klassendefinitionen, bei der Arbeit mit Code First automatisch zu konfigurieren. Die Konventionen werden in den System.Data.Entity.ModelConfiguration.Conventions-Namespace definiert.  

Sie können das Modell weiter mithilfe von datenanmerkungen oder der fluent-API konfigurieren. Die Konfiguration über die fluent-API, gefolgt von datenanmerkungen und Konventionen erhält Vorrang vor. Weitere Informationen finden Sie unter [Datenanmerkungen](~/ef6/modeling/code-first/data-annotations.md), [Fluent-API – Beziehungen](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent-API - Typen und Eigenschaften, die](~/ef6/modeling/code-first/fluent/types-and-properties.md) und [Fluent-API in VB.NET](~/ef6/modeling/code-first/fluent/vb.md).  

Eine detaillierte Liste mit Code First-Konventionen finden Sie in der [-API-Dokumentation](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx). Dieses Thema enthält eine Übersicht über die Konventionen, die von Code First verwendet.  

## <a name="type-discovery"></a>Typermittlung  

Bei Verwendung von Code First-Entwicklung beginnen Sie in der Regel durch das Schreiben von .NET Framework-Klassen, die das konzeptionelle (Domäne)-Modell zu definieren. Zusätzlich zum Definieren von Klassen, müssen Sie auch können **"DbContext"** wissen, welche Typen im Modell enthalten sein sollen. Zu diesem Zweck definieren Sie eine Kontextklasse, die von abgeleitet **"DbContext"** und macht **"DbSet"** Eigenschaften für die Typen, die Teil des Modells sein sollen. Code zuerst diese Typen enthalten, und auch wird per Pullvorgang in Typen auf die verwiesen wird, auch wenn der referenzierten Typen in einer anderen Assembly definiert sind.  

Wenn die Typen in einer Vererbungshierarchie beteiligt, es ist ausreichend, definieren Sie eine **"DbSet"** -Eigenschaft für die Basisklasse und abgeleiteten Typen werden automatisch eingeschlossen, wenn sie in der gleichen Assembly wie der Basisklasse sind.  

Im folgenden Beispiel ist nur ein **"DbSet"** -Eigenschaft definiert, auf die **SchoolEntities** Klasse (**Abteilungen**). Code wird diese Eigenschaft zunächst ermitteln und alle referenzierten Typen an. beziehen.  

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

Wenn Sie einen Typ aus dem Modell ausschließen möchten, verwenden Sie die **NotMapped** Attribut oder das **DbModelBuilder.Ignore** fluent-API.  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a>Primary Key-Konvention  

Code leitet zunächst, dass eine Eigenschaft ein primärer Schlüssel ist, wenn eine Eigenschaft einer Klasse "ID" (ohne Beachtung von Groß-/Kleinschreibung heißt) oder der Klassennamen gefolgt von "ID". Wenn der Typ der Eigenschaft des primären Schlüssels numerisch ist oder GUID werden als eine Identity-Spalte konfiguriert.  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a>Beziehung Konvention  

Entity Framework bieten Ihnen eine Möglichkeit, eine Beziehung zwischen zwei Entitätstypen zu navigieren Navigationseigenschaften. Jedes Objekt kann für jede Beziehung, an der es beteiligt ist, über eine Navigationseigenschaft verfügen. Navigationseigenschaften können Sie zum Navigieren und Verwalten von Beziehungen in beide Richtungen, entweder ein Verweisobjekt zurückgegeben (wenn die Multiplizität entweder nacheinander oder 0 (null) oder 1) oder eine Sammlung (wenn die Multiplizität n ist). Zuerst leitet Code Beziehungen basierend auf die in Ihre Typen definierten Navigationseigenschaften ab.  

Zusätzlich zu Navigationseigenschaften empfehlen wir, dass Sie Fremdschlüsseleigenschaften für die Typen enthalten, die abhängigen Objekte darstellen. Eine Eigenschaft mit dem gleichen Datentyp wie der Dienstprinzipal Primärschlüsseleigenschaft und mit einem Namen, der auf einem der folgenden Formate folgt, stellt einen Fremdschlüssel für die Beziehung: "\<navigationseigenschaftennamen\>\<Dienstprinzipal Primärschlüsseleigenschaft Namen\>','\<Prinzipalklasse Namen\>\<Primärschlüsseleigenschaft Namen\>", oder"\<principal Primärschlüsseleigenschaft Namen\>". Wenn mehrere Übereinstimmungen gefunden werden, und klicken Sie dann Vorrang vor in der obigen Reihenfolge angegeben ist. Foreign Key-Erkennung ist nicht in der Groß-/Kleinschreibung beachtet. Wenn eine Fremdschlüsseleigenschaft erkannt wird, leitet Code First die Multiplizität der Beziehung basierend auf der NULL-Zulässigkeit des Fremdschlüssels. Wenn die Eigenschaft auf NULL festlegbar ist. ist die Beziehung als optional registrierten; Andernfalls wird die Beziehung registriert nach Bedarf.  

Ist ein Fremdschlüssel für die abhängige Entität nicht NULL-Werte zulässt, legt dann Code First Löschweitergabe für die Beziehung. Wenn ein Fremdschlüssel in der abhängigen Entität NULL-Werte zulässt ist, Code First Löschweitergabe nicht auf der Beziehung fest, und wenn der Prinzipal gelöscht wird die foreign Key wird festgelegt auf Null. Löschen Sie die Multiplizität und Cascade Verhalten von erkannten Konvention kann mithilfe der fluent-API überschrieben werden.  

Im folgenden Beispiel werden die Navigationseigenschaften und ein Fremdschlüssel verwendet, zum Definieren der Beziehung zwischen den Klassen-Abteilung und Kurs.  

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
> Wenn Sie über mehrere Beziehungen zwischen den gleichen Typen verfügen (Nehmen wir beispielsweise an, die Sie definieren die **Person** und **Buch** Klassen, in denen die **Person** -Klasse enthält die  **ReviewedBooks** und **AuthoredBooks** Navigationseigenschaften und **Buch** -Klasse enthält die **Autor** und  **Prüfer** Navigationseigenschaften) müssen Sie die Beziehungen manuell zu konfigurieren, mithilfe von Datenanmerkungen oder der fluent-API. Weitere Informationen finden Sie unter [Datenanmerkungen - Beziehungen](~/ef6/modeling/code-first/data-annotations.md) und [Fluent-API – Beziehungen](~/ef6/modeling/code-first/fluent/relationships.md).  

## <a name="complex-types-convention"></a>Konvention für komplexe Typen  

Wenn Code First eine Klassendefinition ermittelt, in dem ein primärer Schlüssel kann nicht abgeleitet werden, und keinen primärer Schlüssel mithilfe von datenanmerkungen oder der fluent-API registriert ist, wird der Typ als komplexen Typ automatisch registriert. Erkennung von komplexen Typs erfordert auch, dass der Typ keine Eigenschaften besitzt, die Entitätstypen verweisen und nicht aus einer Auflistungseigenschaft auf einen anderen Typ verwiesen wird. Erhalten die folgenden Klassendefinitionen Code First würde abzuleiten, die **Details** ist ein komplexer Typ, da es keinen Primärschlüssel aufweist.  

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

## <a name="connection-string-convention"></a>Verbindung Zeichenfolgenkonventionen  

Die Konventionen erfahren Sie, dass die Verbindung finden Sie unter ermittelt mithilfe von "DbContext" [Verbindungen und Modelle](~/ef6/fundamentals/configuring/connection-strings.md).  

## <a name="removing-conventions"></a>Entfernen von Konventionen  

Sie können die Konventionen, die im Namespace System.Data.Entity.ModelConfiguration.Conventions definierten entfernen. Im folgenden Beispiel wird **PluralizingTableNameConvention**.  

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

## <a name="custom-conventions"></a>Benutzerdefinierte Konventionen  

Benutzerdefinierte Konventionen werden in EF6 oder höher unterstützt. Weitere Informationen finden Sie unter [benutzerdefinierte Code First-Konventionen](~/ef6/modeling/code-first/conventions/custom.md).
