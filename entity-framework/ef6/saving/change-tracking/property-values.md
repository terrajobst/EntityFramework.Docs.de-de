---
title: Arbeiten mit Eigenschaftswerten - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e3278b4b-9378-4fdb-923d-f64d80aaae70
ms.openlocfilehash: afde503bb4ed15fcf83a57053541cd5da8c89835
ms.sourcegitcommit: 50521b4a2f71139e6a7210a69ac73da582ef46cf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/27/2019
ms.locfileid: "67416675"
---
# <a name="working-with-property-values"></a>Verwenden von Eigenschaftswerten
In den meisten Fällen übernimmt Entity Framework der nachverfolgung von Status, die ursprünglichen Werte und die aktuellen Werte der Eigenschaften der Entitätsinstanzen. Möglicherweise gibt es jedoch einige Fälle – z. B. nicht verbundenen Szenarien – angezeigt oder bearbeitet, die EF bietet Informationen zu den Eigenschaften werden sollen. Die in diesem Thema dargestellten Techniken gelten jeweils für Modelle, die mit Code First und dem EF-Designer erstellt wurden.  

Entitätsframework verfolgt des zwei Werte für jede Eigenschaft einer verfolgten Entität. Der aktuelle Wert ist, wie der Name, den aktuellen Wert der Eigenschaft in der Entität. Der ursprüngliche Wert ist der Wert, den die Eigenschaft hätte, wenn die Entität aus der Datenbank abgefragt oder an den Kontext angefügt wurde.  

Es gibt zwei allgemeine Mechanismen für die Arbeit mit Eigenschaftswerten aus:  

- Der Wert einer einzelnen Eigenschaft kann auf eine stark typisierte Weise mithilfe der Methode für die Eigenschaft abgerufen werden.  
- Werte für alle Eigenschaften einer Entität können in ein DbPropertyValues-Objekt gelesen werden. DbPropertyValues stellt dann eine wörterbuchähnliche-Objekt können Sie Eigenschaftswerte zum Lesen und festlegen. Die Werte in einem DbPropertyValues-Objekt können von Werten in einem anderen DbPropertyValues-Objekt oder von Werten in ein anderes Objekt, z. B. eine weitere Kopie der Entität oder ein einfaches datentransferobjekt (DTO) festgelegt werden.  

In den folgenden Abschnitten sind Beispiele für beide der oben genannten Mechanismen an.  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a>Abrufen und Festlegen des aktuellen oder den ursprünglichen Werts einer einzelnen Eigenschaft  

Das folgende Beispiel zeigt, wie der aktuelle Wert einer Eigenschaft zu lesen und klicken Sie dann auf einen neuen Wert festgelegt werden kann:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(3);

    // Read the current value of the Name property
    string currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;

    // Set the Name property to a new value
    context.Entry(blog).Property(u => u.Name).CurrentValue = "My Fancy Blog";

    // Read the current value of the Name property using a string for the property name
    object currentName2 = context.Entry(blog).Property("Name").CurrentValue;

    // Set the Name property to a new value using a string for the property name
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

Verwenden Sie die OriginalValue-Eigenschaft anstelle der CurrentValue-Eigenschaft zu lesen, oder legen Sie den ursprünglichen Wert.  

Beachten Sie, dass der zurückgegebene Wert als "Object" eingegeben wird, wenn eine Zeichenfolge mit dem Namen der Eigenschaft angegeben. Auf der anderen Seite ist der Rückgabewert stark typisiert, wenn ein Lambda-Ausdruck verwendet wird.  

Festlegen des Eigenschaftswerts wie folgt, wird nur die Eigenschaft markieren, als geändert, wenn der neue Wert den alten Wert unterscheidet.  

Wenn ein Eigenschaftswert, auf diese Weise festgelegt ist wird die Änderung automatisch erkannt werden, auch wenn AutoDetectChanges deaktiviert ist.  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a>Abrufen und Festlegen von den aktuellen Wert einer nicht zugeordneten Eigenschaft.  

Der aktuelle Wert einer Eigenschaft, die nicht in der Datenbank zugeordnet ist, kann auch gelesen werden. Ein Beispiel für eine nicht zugeordnete Eigenschaft ist möglicherweise eine Eigenschaft RssLink-Blog. Dieser Wert kann basierend auf den BlogId berechnet werden und daher nicht in der Datenbank gespeichert werden muss. Zum Beispiel:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    // Read the current value of an unmapped property
    var rssLink = context.Entry(blog).Property(p => p.RssLink).CurrentValue;

    // Use a string to specify the property name
    var rssLinkAgain = context.Entry(blog).Property("RssLink").CurrentValue;
}
```  

Der aktuelle Wert kann auch festgelegt werden, wenn die Eigenschaft einen Setter verfügbar macht.  

Lesen die Werte der Eigenschaften, die nicht zugeordneten ist nützlich, beim Durchführen von Entity Framework-Überprüfung von nicht zugeordneten Eigenschaften. Aus demselben Grund können aktuelle Werte lesen und für die Eigenschaften von Entitäten, die aktuell nicht vom Kontext verfolgt werden eingestellt werden. Zum Beispiel:  

``` csharp
using (var context = new BloggingContext())
{
    // Create an entity that is not being tracked
    var blog = new Blog { Name = "ADO.NET Blog" };

    // Read and set the current value of Name as before
    var currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;
    context.Entry(blog).Property(u => u.Name).CurrentValue = "My Fancy Blog";
    var currentName2 = context.Entry(blog).Property("Name").CurrentValue;
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

Beachten Sie, dass die ursprünglichen Werte sind nicht verfügbar, für nicht zugeordnete Eigenschaften oder Eigenschaften von Entitäten, die nicht vom Kontext nachverfolgt werden.  

## <a name="checking-whether-a-property-is-marked-as-modified"></a>Überprüft, ob eine Eigenschaft als geändert markiert ist  

Das folgende Beispiel zeigt, wie Sie überprüfen, und zwar unabhängig davon, ob eine einzelne Eigenschaft als geändert markiert ist:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

Die Werte der geänderten Eigenschaften sind als Updates an die Datenbank gesendet, wenn SaveChanges aufgerufen wird.  

##  <a name="marking-a-property-as-modified"></a>Eine Eigenschaft als geändert markieren  

Das folgende Beispiel zeigt, wie Sie erzwingen, dass für eine einzelne Eigenschaft als geändert markiert werden:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

Markieren eine Eigenschaft als geänderte erzwingt ein Update für werden an die Datenbank für die Eigenschaft aus, wenn SaveChanges aufgerufen wird, selbst wenn der aktuelle Wert der Eigenschaft den ursprünglichen Wert identisch ist.  

Es ist nicht aktuell möglich ist, zum Zurücksetzen einer einzelnen Eigenschaft nicht geändert werden, nachdem er als geändert gekennzeichnet wurde. Dies ist etwas, das wir in einer zukünftigen Version unterstützt werden soll.  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a>Beim Lesen der aktuellen und ursprünglichen Datenbankwerte für alle Eigenschaften einer Entität  

Das folgende Beispiel zeigt, wie Sie die aktuellen Werte, die ursprünglichen Werte und die Werte tatsächlich in der Datenbank für alle zugeordneten Eigenschaften einer Entität zu lesen.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Make a modification to Name in the tracked entity
    blog.Name = "My Cool Blog";

    // Make a modification to Name in the database
    context.Database.SqlCommand("update dbo.Blogs set Name = 'My Boring Blog' where Id = 1");

    // Print out current, original, and database values
    Console.WriteLine("Current values:");
    PrintValues(context.Entry(blog).CurrentValues);

    Console.WriteLine("\nOriginal values:");
    PrintValues(context.Entry(blog).OriginalValues);

    Console.WriteLine("\nDatabase values:");
    PrintValues(context.Entry(blog).GetDatabaseValues());
}

public static void PrintValues(DbPropertyValues values)
{
    foreach (var propertyName in values.PropertyNames)
    {
        Console.WriteLine("Property {0} has value {1}",
                          propertyName, values[propertyName]);
    }
}
```  

Die aktuellen Werte sind die Werte, die die Eigenschaften der Entität derzeit enthalten. Die ursprünglichen Werte werden die Werte, die aus der Datenbank gelesen wurden, wenn die Entität abgefragt wurde. Die Datenbankwerte sind die Werte, da sie derzeit in der Datenbank gespeichert werden. Abrufen der Datenbankwerte ist nützlich, wenn die Werte in der Datenbank geändert haben können, da die Entität abgefragt wurde, wie z. B. wenn eine gleichzeitige bearbeiten, um die Datenbank von einem anderen Benutzer vorgenommen wurde.  

## <a name="setting-current-or-original-values-from-another-object"></a>Festlegen der aktuelle oder den ursprüngliche Werten aus einem anderen Objekt  

Die aktuelle oder ursprüngliche Werte einer verfolgten Entität können durch Kopieren der Werte aus einem anderen Objekt aktualisiert werden. Zum Beispiel:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var coolBlog = new Blog { Id = 1, Name = "My Cool Blog" };
    var boringBlog = new BlogDto { Id = 1, Name = "My Boring Blog" };

    // Change the current and original values by copying the values from other objects
    var entry = context.Entry(blog);
    entry.CurrentValues.SetValues(coolBlog);
    entry.OriginalValues.SetValues(boringBlog);

    // Print out current and original values
    Console.WriteLine("Current values:");
    PrintValues(entry.CurrentValues);

    Console.WriteLine("\nOriginal values:");
    PrintValues(entry.OriginalValues);
}

public class BlogDto
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```  

Ausführen des obigen Codes werden ausgeben:  

```  
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

Diese Technik wird manchmal verwendet, bei der Aktualisierung einer Entitätstyps mit den Werten, die von einem Dienstaufruf oder einem Client in eine n-schichtige Anwendung abgerufen. Beachten Sie, dass das Objekt, nicht desselben Typs wie die Entität zu werden unbedingt, solange es Eigenschaften verfügt, deren Namen mit denen der Entität entsprechen. Im obigen Beispiel wird eine Instanz von BlogDTO zum Aktualisieren der ursprünglichen Werte verwendet.  

Beachten Sie, dass nur die Eigenschaften, die auf unterschiedliche Werte beim Kopieren aus dem anderen Objekt festgelegt werden als geändert markiert werden.  

## <a name="setting-current-or-original-values-from-a-dictionary"></a>Festlegen der aktuelle oder den ursprüngliche Werten aus einem Wörterbuch  

Die aktuelle oder ursprüngliche Werte einer verfolgten Entität können durch Kopieren von Werten aus einem Wörterbuch oder eine andere Datenstruktur aktualisiert werden. Zum Beispiel:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var newValues = new Dictionary\<string, object>
    {
        { "Name", "The New ADO.NET Blog" },
        { "Url", "blogs.msdn.com/adonet" },
    };

    var currentValues = context.Entry(blog).CurrentValues;

    foreach (var propertyName in newValues.Keys)
    {
        currentValues[propertyName] = newValues[propertyName];
    }

    PrintValues(currentValues);
}
```  

Verwenden Sie die OriginalValues-Eigenschaft anstelle der CurrentValues-Eigenschaft, um ursprüngliche Werte festzulegen.  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a>Festlegen der aktuelle oder den ursprüngliche Werten aus einem Wörterbuch mit der Eigenschaft  

Eine Alternative zur Verwendung von CurrentValues oder OriginalValues wie oben gezeigt ist die Verwendung die Property-Methode zum Festlegen des Werts für jede Eigenschaft. Dies kann besser sein, wenn Sie die Werte von komplexen Eigenschaften festlegen müssen. Zum Beispiel:  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    var newValues = new Dictionary\<string, object>
    {
        { "Name", "John Doe" },
        { "Location.City", "Redmond" },
        { "Location.State.Name", "Washington" },
        { "Location.State.Code", "WA" },
    };

    var entry = context.Entry(user);

    foreach (var propertyName in newValues.Keys)
    {
        entry.Property(propertyName).CurrentValue = newValues[propertyName];
    }
}
```  

Im Beispiel oben komplexe Eigenschaften erfolgt mithilfe von Namen in punktierter Schreibweise. Weitere Informationen zum Zugriff auf komplexe Eigenschaften finden Sie unter den beiden Abschnitten unten in diesem Thema, insbesondere zu komplexen Eigenschaften zu erhalten.  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a>Erstellen ein geklontes Objekt, das mit Datenbankwerte, ursprüngliche oder aktuelle  

Das Objekt DbPropertyValues Merry CurrentValues, OriginalValues, oder GetDatabaseValues können verwendet werden, um einen Klon der Entität zu erstellen. Dieser Klon enthält die Eigenschaftswerte aus dem DbPropertyValues-Objekt verwendet, um es zu erstellen. Zum Beispiel:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

Beachten Sie, dass das zurückgegebene Objekt nicht die Entität und nicht vom Kontext nachverfolgt wird. Das zurückgegebene Objekt muss auch keine Beziehungen zu anderen Objekten festgelegt.  

Das geklonte Objekt möglich nützlich zum Lösen von Problemen im Zusammenhang mit gleichzeitiger Aktualisierungen der Datenbank, insbesondere, in denen eine Benutzeroberfläche, die die Datenbindung an Objekte eines bestimmten Typs umfasst verwendet wird.  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a>Abrufen und Festlegen der aktuellen oder den ursprünglichen Werten der Eigenschaften, die komplexe  

Der Wert eines ganzen komplexen Objekts kann sein, lesen und mit der Property-Methode, so wie es für eine primitive Eigenschaft sein kann. Darüber hinaus können Sie das komplexe Objekt und gelesen oder festgelegt Eigenschaften dieses Objekts, oder sogar ein geschachteltes Objekt aufgliedern. Hier einige Beispiele:  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    // Get the Location complex object
    var location = context.Entry(user)
                       .Property(u => u.Location)
                       .CurrentValue;

    // Get the nested State complex object using chained calls
    var state1 = context.Entry(user)
                     .ComplexProperty(u => u.Location)
                     .Property(l => l.State)
                     .CurrentValue;

    // Get the nested State complex object using a single lambda expression
    var state2 = context.Entry(user)
                     .Property(u => u.Location.State)
                     .CurrentValue;

    // Get the nested State complex object using a dotted string
    var state3 = context.Entry(user)
                     .Property("Location.State")
                     .CurrentValue;

    // Get the value of the Name property on the nested State complex object using chained calls
    var name1 = context.Entry(user)
                       .ComplexProperty(u => u.Location)
                       .ComplexProperty(l => l.State)
                       .Property(s => s.Name)
                       .CurrentValue;

    // Get the value of the Name property on the nested State complex object using a single lambda expression
    var name2 = context.Entry(user)
                       .Property(u => u.Location.State.Name)
                       .CurrentValue;

    // Get the value of the Name property on the nested State complex object using a dotted string
    var name3 = context.Entry(user)
                       .Property("Location.State.Name")
                       .CurrentValue;
}
```  

Verwenden Sie den OriginalValue-Eigenschaft anstelle der CurrentValue-Eigenschaft, um einen ursprünglichen Wert abgerufen oder festgelegt.  

Beachten Sie, dass entweder die Eigenschaft oder der ComplexProperty-Methode verwendet werden kann, um eine komplexe Eigenschaft zuzugreifen. Allerdings muss die ComplexProperty-Methode verwendet werden, wenn Sie einen Drilldown in das komplexe Objekt mit zusätzlichen Eigenschaft möchten oder ComplexProperty aufruft.  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a>Verwenden den Zugriff auf komplexe Eigenschaften DbPropertyValues  

Wenn Sie CurrentValues OriginalValues oder GetDatabaseValues verwenden, um die aktuelle ursprüngliche zu erhalten, oder Datenbankwerte für eine Entität, die Werte der komplexen Eigenschaften werden als geschachtelte DbPropertyValues Objekte zurückgegeben. Diese geschachtelte Objekte kann dann verwendet werden, um die Werte des komplexen Objekts zu erhalten. Beispielsweise wird die folgende Methode, die Werte aller Eigenschaften, einschließlich der Werte von komplexen Eigenschaften und verschachtelte komplexe Eigenschaften ausgegeben.  

``` csharp
public static void WritePropertyValues(string parentPropertyName, DbPropertyValues propertyValues)
{
    foreach (var propertyName in propertyValues.PropertyNames)
    {
        var nestedValues = propertyValues[propertyName] as DbPropertyValues;
        if (nestedValues != null)
        {
            WritePropertyValues(parentPropertyName + propertyName + ".", nestedValues);
        }
        else
        {
            Console.WriteLine("Property {0}{1} has value {2}",
                              parentPropertyName, propertyName,
                              propertyValues[propertyName]);
        }
    }
}
```  

Um die Methode alle aktuellen Eigenschaftswerte auszudrucken würde wie folgt aufgerufen werden:  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
