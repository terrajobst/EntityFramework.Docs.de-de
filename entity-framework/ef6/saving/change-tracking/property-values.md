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
# <a name="working-with-property-values"></a><span data-ttu-id="d205a-102">Verwenden von Eigenschaftswerten</span><span class="sxs-lookup"><span data-stu-id="d205a-102">Working with property values</span></span>
<span data-ttu-id="d205a-103">In den meisten Fällen übernimmt Entity Framework der nachverfolgung von Status, die ursprünglichen Werte und die aktuellen Werte der Eigenschaften der Entitätsinstanzen.</span><span class="sxs-lookup"><span data-stu-id="d205a-103">For the most part Entity Framework will take care of tracking the state, original values, and current values of the properties of your entity instances.</span></span> <span data-ttu-id="d205a-104">Möglicherweise gibt es jedoch einige Fälle – z. B. nicht verbundenen Szenarien – angezeigt oder bearbeitet, die EF bietet Informationen zu den Eigenschaften werden sollen.</span><span class="sxs-lookup"><span data-stu-id="d205a-104">However, there may be some cases - such as disconnected scenarios - where you want to view or manipulate the information EF has about the properties.</span></span> <span data-ttu-id="d205a-105">Die in diesem Thema dargestellten Techniken gelten jeweils für Modelle, die mit Code First und dem EF-Designer erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="d205a-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="d205a-106">Entitätsframework verfolgt des zwei Werte für jede Eigenschaft einer verfolgten Entität.</span><span class="sxs-lookup"><span data-stu-id="d205a-106">Entity Framework keeps track of two values for each property of a tracked entity.</span></span> <span data-ttu-id="d205a-107">Der aktuelle Wert ist, wie der Name, den aktuellen Wert der Eigenschaft in der Entität.</span><span class="sxs-lookup"><span data-stu-id="d205a-107">The current value is, as the name indicates, the current value of the property in the entity.</span></span> <span data-ttu-id="d205a-108">Der ursprüngliche Wert ist der Wert, den die Eigenschaft hätte, wenn die Entität aus der Datenbank abgefragt oder an den Kontext angefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="d205a-108">The original value is the value that the property had when the entity was queried from the database or attached to the context.</span></span>  

<span data-ttu-id="d205a-109">Es gibt zwei allgemeine Mechanismen für die Arbeit mit Eigenschaftswerten aus:</span><span class="sxs-lookup"><span data-stu-id="d205a-109">There are two general mechanisms for working with property values:</span></span>  

- <span data-ttu-id="d205a-110">Der Wert einer einzelnen Eigenschaft kann auf eine stark typisierte Weise mithilfe der Methode für die Eigenschaft abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="d205a-110">The value of a single property can be obtained in a strongly typed way using the Property method.</span></span>  
- <span data-ttu-id="d205a-111">Werte für alle Eigenschaften einer Entität können in ein DbPropertyValues-Objekt gelesen werden.</span><span class="sxs-lookup"><span data-stu-id="d205a-111">Values for all properties of an entity can be read into a DbPropertyValues object.</span></span> <span data-ttu-id="d205a-112">DbPropertyValues stellt dann eine wörterbuchähnliche-Objekt können Sie Eigenschaftswerte zum Lesen und festlegen.</span><span class="sxs-lookup"><span data-stu-id="d205a-112">DbPropertyValues then acts as a dictionary-like object to allow property values to be read and set.</span></span> <span data-ttu-id="d205a-113">Die Werte in einem DbPropertyValues-Objekt können von Werten in einem anderen DbPropertyValues-Objekt oder von Werten in ein anderes Objekt, z. B. eine weitere Kopie der Entität oder ein einfaches datentransferobjekt (DTO) festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="d205a-113">The values in a DbPropertyValues object can be set from values in another DbPropertyValues object or from values in some other object, such as another copy of the entity or a simple data transfer object (DTO).</span></span>  

<span data-ttu-id="d205a-114">In den folgenden Abschnitten sind Beispiele für beide der oben genannten Mechanismen an.</span><span class="sxs-lookup"><span data-stu-id="d205a-114">The sections below show examples of using both of the above mechanisms.</span></span>  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a><span data-ttu-id="d205a-115">Abrufen und Festlegen des aktuellen oder den ursprünglichen Werts einer einzelnen Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="d205a-115">Getting and setting the current or original value of an individual property</span></span>  

<span data-ttu-id="d205a-116">Das folgende Beispiel zeigt, wie der aktuelle Wert einer Eigenschaft zu lesen und klicken Sie dann auf einen neuen Wert festgelegt werden kann:</span><span class="sxs-lookup"><span data-stu-id="d205a-116">The example below shows how the current value of a property can be read and then set to a new value:</span></span>  

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

<span data-ttu-id="d205a-117">Verwenden Sie die OriginalValue-Eigenschaft anstelle der CurrentValue-Eigenschaft zu lesen, oder legen Sie den ursprünglichen Wert.</span><span class="sxs-lookup"><span data-stu-id="d205a-117">Use the OriginalValue property instead of the CurrentValue property to read or set the original value.</span></span>  

<span data-ttu-id="d205a-118">Beachten Sie, dass der zurückgegebene Wert als "Object" eingegeben wird, wenn eine Zeichenfolge mit dem Namen der Eigenschaft angegeben.</span><span class="sxs-lookup"><span data-stu-id="d205a-118">Note that the returned value is typed as “object” when a string is used to specify the property name.</span></span> <span data-ttu-id="d205a-119">Auf der anderen Seite ist der Rückgabewert stark typisiert, wenn ein Lambda-Ausdruck verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="d205a-119">On the other hand, the returned value is strongly typed if a lambda expression is used.</span></span>  

<span data-ttu-id="d205a-120">Festlegen des Eigenschaftswerts wie folgt, wird nur die Eigenschaft markieren, als geändert, wenn der neue Wert den alten Wert unterscheidet.</span><span class="sxs-lookup"><span data-stu-id="d205a-120">Setting the property value like this will only mark the property as modified if the new value is different from the old value.</span></span>  

<span data-ttu-id="d205a-121">Wenn ein Eigenschaftswert, auf diese Weise festgelegt ist wird die Änderung automatisch erkannt werden, auch wenn AutoDetectChanges deaktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="d205a-121">When a property value is set in this way the change is automatically detected even if AutoDetectChanges is turned off.</span></span>  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a><span data-ttu-id="d205a-122">Abrufen und Festlegen von den aktuellen Wert einer nicht zugeordneten Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="d205a-122">Getting and setting the current value of an unmapped property</span></span>  

<span data-ttu-id="d205a-123">Der aktuelle Wert einer Eigenschaft, die nicht in der Datenbank zugeordnet ist, kann auch gelesen werden.</span><span class="sxs-lookup"><span data-stu-id="d205a-123">The current value of a property that is not mapped to the database can also be read.</span></span> <span data-ttu-id="d205a-124">Ein Beispiel für eine nicht zugeordnete Eigenschaft ist möglicherweise eine Eigenschaft RssLink-Blog.</span><span class="sxs-lookup"><span data-stu-id="d205a-124">An example of an unmapped property could be an RssLink property on Blog.</span></span> <span data-ttu-id="d205a-125">Dieser Wert kann basierend auf den BlogId berechnet werden und daher nicht in der Datenbank gespeichert werden muss.</span><span class="sxs-lookup"><span data-stu-id="d205a-125">This value may be calculated based on the BlogId, and therefore doesn't need to be stored in the database.</span></span> <span data-ttu-id="d205a-126">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d205a-126">For example:</span></span>  

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

<span data-ttu-id="d205a-127">Der aktuelle Wert kann auch festgelegt werden, wenn die Eigenschaft einen Setter verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="d205a-127">The current value can also be set if the property exposes a setter.</span></span>  

<span data-ttu-id="d205a-128">Lesen die Werte der Eigenschaften, die nicht zugeordneten ist nützlich, beim Durchführen von Entity Framework-Überprüfung von nicht zugeordneten Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="d205a-128">Reading the values of unmapped properties is useful when performing Entity Framework validation of unmapped properties.</span></span> <span data-ttu-id="d205a-129">Aus demselben Grund können aktuelle Werte lesen und für die Eigenschaften von Entitäten, die aktuell nicht vom Kontext verfolgt werden eingestellt werden.</span><span class="sxs-lookup"><span data-stu-id="d205a-129">For the same reason current values can be read and set for properties of entities that are not currently being tracked by the context.</span></span> <span data-ttu-id="d205a-130">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d205a-130">For example:</span></span>  

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

<span data-ttu-id="d205a-131">Beachten Sie, dass die ursprünglichen Werte sind nicht verfügbar, für nicht zugeordnete Eigenschaften oder Eigenschaften von Entitäten, die nicht vom Kontext nachverfolgt werden.</span><span class="sxs-lookup"><span data-stu-id="d205a-131">Note that original values are not available for unmapped properties or for properties of entities that are not being tracked by the context.</span></span>  

## <a name="checking-whether-a-property-is-marked-as-modified"></a><span data-ttu-id="d205a-132">Überprüft, ob eine Eigenschaft als geändert markiert ist</span><span class="sxs-lookup"><span data-stu-id="d205a-132">Checking whether a property is marked as modified</span></span>  

<span data-ttu-id="d205a-133">Das folgende Beispiel zeigt, wie Sie überprüfen, und zwar unabhängig davon, ob eine einzelne Eigenschaft als geändert markiert ist:</span><span class="sxs-lookup"><span data-stu-id="d205a-133">The example below shows how to check whether or not an individual property is marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

<span data-ttu-id="d205a-134">Die Werte der geänderten Eigenschaften sind als Updates an die Datenbank gesendet, wenn SaveChanges aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="d205a-134">The values of modified properties are sent as updates to the database when SaveChanges is called.</span></span>  

##  <a name="marking-a-property-as-modified"></a><span data-ttu-id="d205a-135">Eine Eigenschaft als geändert markieren</span><span class="sxs-lookup"><span data-stu-id="d205a-135">Marking a property as modified</span></span>  

<span data-ttu-id="d205a-136">Das folgende Beispiel zeigt, wie Sie erzwingen, dass für eine einzelne Eigenschaft als geändert markiert werden:</span><span class="sxs-lookup"><span data-stu-id="d205a-136">The example below shows how to force an individual property to be marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

<span data-ttu-id="d205a-137">Markieren eine Eigenschaft als geänderte erzwingt ein Update für werden an die Datenbank für die Eigenschaft aus, wenn SaveChanges aufgerufen wird, selbst wenn der aktuelle Wert der Eigenschaft den ursprünglichen Wert identisch ist.</span><span class="sxs-lookup"><span data-stu-id="d205a-137">Marking a property as modified forces an update to be send to the database for the property when SaveChanges is called even if the current value of the property is the same as its original value.</span></span>  

<span data-ttu-id="d205a-138">Es ist nicht aktuell möglich ist, zum Zurücksetzen einer einzelnen Eigenschaft nicht geändert werden, nachdem er als geändert gekennzeichnet wurde.</span><span class="sxs-lookup"><span data-stu-id="d205a-138">It is not currently possible to reset an individual property to be not modified after it has been marked as modified.</span></span> <span data-ttu-id="d205a-139">Dies ist etwas, das wir in einer zukünftigen Version unterstützt werden soll.</span><span class="sxs-lookup"><span data-stu-id="d205a-139">This is something we plan to support in a future release.</span></span>  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a><span data-ttu-id="d205a-140">Beim Lesen der aktuellen und ursprünglichen Datenbankwerte für alle Eigenschaften einer Entität</span><span class="sxs-lookup"><span data-stu-id="d205a-140">Reading current, original, and database values for all properties of an entity</span></span>  

<span data-ttu-id="d205a-141">Das folgende Beispiel zeigt, wie Sie die aktuellen Werte, die ursprünglichen Werte und die Werte tatsächlich in der Datenbank für alle zugeordneten Eigenschaften einer Entität zu lesen.</span><span class="sxs-lookup"><span data-stu-id="d205a-141">The example below shows how to read the current values, the original values, and the values actually in the database for all mapped properties of an entity.</span></span>  

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

<span data-ttu-id="d205a-142">Die aktuellen Werte sind die Werte, die die Eigenschaften der Entität derzeit enthalten.</span><span class="sxs-lookup"><span data-stu-id="d205a-142">The current values are the values that the properties of the entity currently contain.</span></span> <span data-ttu-id="d205a-143">Die ursprünglichen Werte werden die Werte, die aus der Datenbank gelesen wurden, wenn die Entität abgefragt wurde.</span><span class="sxs-lookup"><span data-stu-id="d205a-143">The original values are the values that were read from the database when the entity was queried.</span></span> <span data-ttu-id="d205a-144">Die Datenbankwerte sind die Werte, da sie derzeit in der Datenbank gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="d205a-144">The database values are the values as they are currently stored in the database.</span></span> <span data-ttu-id="d205a-145">Abrufen der Datenbankwerte ist nützlich, wenn die Werte in der Datenbank geändert haben können, da die Entität abgefragt wurde, wie z. B. wenn eine gleichzeitige bearbeiten, um die Datenbank von einem anderen Benutzer vorgenommen wurde.</span><span class="sxs-lookup"><span data-stu-id="d205a-145">Getting the database values is useful when the values in the database may have changed since the entity was queried such as when a concurrent edit to the database has been made by another user.</span></span>  

## <a name="setting-current-or-original-values-from-another-object"></a><span data-ttu-id="d205a-146">Festlegen der aktuelle oder den ursprüngliche Werten aus einem anderen Objekt</span><span class="sxs-lookup"><span data-stu-id="d205a-146">Setting current or original values from another object</span></span>  

<span data-ttu-id="d205a-147">Die aktuelle oder ursprüngliche Werte einer verfolgten Entität können durch Kopieren der Werte aus einem anderen Objekt aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="d205a-147">The current or original values of a tracked entity can be updated by copying values from another object.</span></span> <span data-ttu-id="d205a-148">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d205a-148">For example:</span></span>  

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

<span data-ttu-id="d205a-149">Ausführen des obigen Codes werden ausgeben:</span><span class="sxs-lookup"><span data-stu-id="d205a-149">Running the code above will print out:</span></span>  

```  
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

<span data-ttu-id="d205a-150">Diese Technik wird manchmal verwendet, bei der Aktualisierung einer Entitätstyps mit den Werten, die von einem Dienstaufruf oder einem Client in eine n-schichtige Anwendung abgerufen.</span><span class="sxs-lookup"><span data-stu-id="d205a-150">This technique is sometimes used when updating an entity with values obtained from a service call or a client in an n-tier application.</span></span> <span data-ttu-id="d205a-151">Beachten Sie, dass das Objekt, nicht desselben Typs wie die Entität zu werden unbedingt, solange es Eigenschaften verfügt, deren Namen mit denen der Entität entsprechen.</span><span class="sxs-lookup"><span data-stu-id="d205a-151">Note that the object used does not have to be of the same type as the entity so long as it has properties whose names match those of the entity.</span></span> <span data-ttu-id="d205a-152">Im obigen Beispiel wird eine Instanz von BlogDTO zum Aktualisieren der ursprünglichen Werte verwendet.</span><span class="sxs-lookup"><span data-stu-id="d205a-152">In the example above, an instance of BlogDTO is used to update the original values.</span></span>  

<span data-ttu-id="d205a-153">Beachten Sie, dass nur die Eigenschaften, die auf unterschiedliche Werte beim Kopieren aus dem anderen Objekt festgelegt werden als geändert markiert werden.</span><span class="sxs-lookup"><span data-stu-id="d205a-153">Note that only properties that are set to different values when copied from the other object will be marked as modified.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary"></a><span data-ttu-id="d205a-154">Festlegen der aktuelle oder den ursprüngliche Werten aus einem Wörterbuch</span><span class="sxs-lookup"><span data-stu-id="d205a-154">Setting current or original values from a dictionary</span></span>  

<span data-ttu-id="d205a-155">Die aktuelle oder ursprüngliche Werte einer verfolgten Entität können durch Kopieren von Werten aus einem Wörterbuch oder eine andere Datenstruktur aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="d205a-155">The current or original values of a tracked entity can be updated by copying values from a dictionary or some other data structure.</span></span> <span data-ttu-id="d205a-156">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d205a-156">For example:</span></span>  

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

<span data-ttu-id="d205a-157">Verwenden Sie die OriginalValues-Eigenschaft anstelle der CurrentValues-Eigenschaft, um ursprüngliche Werte festzulegen.</span><span class="sxs-lookup"><span data-stu-id="d205a-157">Use the OriginalValues property instead of the CurrentValues property to set original values.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a><span data-ttu-id="d205a-158">Festlegen der aktuelle oder den ursprüngliche Werten aus einem Wörterbuch mit der Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="d205a-158">Setting current or original values from a dictionary using Property</span></span>  

<span data-ttu-id="d205a-159">Eine Alternative zur Verwendung von CurrentValues oder OriginalValues wie oben gezeigt ist die Verwendung die Property-Methode zum Festlegen des Werts für jede Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="d205a-159">An alternative to using CurrentValues or OriginalValues as shown above is to use the Property method to set the value of each property.</span></span> <span data-ttu-id="d205a-160">Dies kann besser sein, wenn Sie die Werte von komplexen Eigenschaften festlegen müssen.</span><span class="sxs-lookup"><span data-stu-id="d205a-160">This can be preferable when you need to set the values of complex properties.</span></span> <span data-ttu-id="d205a-161">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d205a-161">For example:</span></span>  

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

<span data-ttu-id="d205a-162">Im Beispiel oben komplexe Eigenschaften erfolgt mithilfe von Namen in punktierter Schreibweise.</span><span class="sxs-lookup"><span data-stu-id="d205a-162">In the example above complex properties are accessed using dotted names.</span></span> <span data-ttu-id="d205a-163">Weitere Informationen zum Zugriff auf komplexe Eigenschaften finden Sie unter den beiden Abschnitten unten in diesem Thema, insbesondere zu komplexen Eigenschaften zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="d205a-163">For other ways to access complex properties see the two sections later in this topic specifically about complex properties.</span></span>  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a><span data-ttu-id="d205a-164">Erstellen ein geklontes Objekt, das mit Datenbankwerte, ursprüngliche oder aktuelle</span><span class="sxs-lookup"><span data-stu-id="d205a-164">Creating a cloned object containing current, original, or database values</span></span>  

<span data-ttu-id="d205a-165">Das Objekt DbPropertyValues Merry CurrentValues, OriginalValues, oder GetDatabaseValues können verwendet werden, um einen Klon der Entität zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d205a-165">The DbPropertyValues object returned from CurrentValues, OriginalValues, or GetDatabaseValues can be used to create a clone of the entity.</span></span> <span data-ttu-id="d205a-166">Dieser Klon enthält die Eigenschaftswerte aus dem DbPropertyValues-Objekt verwendet, um es zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d205a-166">This clone will contain the property values from the DbPropertyValues object used to create it.</span></span> <span data-ttu-id="d205a-167">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d205a-167">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

<span data-ttu-id="d205a-168">Beachten Sie, dass das zurückgegebene Objekt nicht die Entität und nicht vom Kontext nachverfolgt wird.</span><span class="sxs-lookup"><span data-stu-id="d205a-168">Note that the object returned is not the entity and is not being tracked by the context.</span></span> <span data-ttu-id="d205a-169">Das zurückgegebene Objekt muss auch keine Beziehungen zu anderen Objekten festgelegt.</span><span class="sxs-lookup"><span data-stu-id="d205a-169">The returned object also does not have any relationships set to other objects.</span></span>  

<span data-ttu-id="d205a-170">Das geklonte Objekt möglich nützlich zum Lösen von Problemen im Zusammenhang mit gleichzeitiger Aktualisierungen der Datenbank, insbesondere, in denen eine Benutzeroberfläche, die die Datenbindung an Objekte eines bestimmten Typs umfasst verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="d205a-170">The cloned object can be useful for resolving issues related to concurrent updates to the database, especially where a UI that involves data binding to objects of a certain type is being used.</span></span>  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a><span data-ttu-id="d205a-171">Abrufen und Festlegen der aktuellen oder den ursprünglichen Werten der Eigenschaften, die komplexe</span><span class="sxs-lookup"><span data-stu-id="d205a-171">Getting and setting the current or original values of complex properties</span></span>  

<span data-ttu-id="d205a-172">Der Wert eines ganzen komplexen Objekts kann sein, lesen und mit der Property-Methode, so wie es für eine primitive Eigenschaft sein kann.</span><span class="sxs-lookup"><span data-stu-id="d205a-172">The value of an entire complex object can be read and set using the Property method just as it can be for a primitive property.</span></span> <span data-ttu-id="d205a-173">Darüber hinaus können Sie das komplexe Objekt und gelesen oder festgelegt Eigenschaften dieses Objekts, oder sogar ein geschachteltes Objekt aufgliedern.</span><span class="sxs-lookup"><span data-stu-id="d205a-173">In addition you can drill down into the complex object and read or set properties of that object, or even a nested object.</span></span> <span data-ttu-id="d205a-174">Hier einige Beispiele:</span><span class="sxs-lookup"><span data-stu-id="d205a-174">Here are some examples:</span></span>  

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

<span data-ttu-id="d205a-175">Verwenden Sie den OriginalValue-Eigenschaft anstelle der CurrentValue-Eigenschaft, um einen ursprünglichen Wert abgerufen oder festgelegt.</span><span class="sxs-lookup"><span data-stu-id="d205a-175">Use the OriginalValue property instead of the CurrentValue property to get or set an original value.</span></span>  

<span data-ttu-id="d205a-176">Beachten Sie, dass entweder die Eigenschaft oder der ComplexProperty-Methode verwendet werden kann, um eine komplexe Eigenschaft zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="d205a-176">Note that either the Property or the ComplexProperty method can be used to access a complex property.</span></span> <span data-ttu-id="d205a-177">Allerdings muss die ComplexProperty-Methode verwendet werden, wenn Sie einen Drilldown in das komplexe Objekt mit zusätzlichen Eigenschaft möchten oder ComplexProperty aufruft.</span><span class="sxs-lookup"><span data-stu-id="d205a-177">However, the ComplexProperty method must be used if you wish to drill down into the complex object with additional Property or ComplexProperty calls.</span></span>  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a><span data-ttu-id="d205a-178">Verwenden den Zugriff auf komplexe Eigenschaften DbPropertyValues</span><span class="sxs-lookup"><span data-stu-id="d205a-178">Using DbPropertyValues to access complex properties</span></span>  

<span data-ttu-id="d205a-179">Wenn Sie CurrentValues OriginalValues oder GetDatabaseValues verwenden, um die aktuelle ursprüngliche zu erhalten, oder Datenbankwerte für eine Entität, die Werte der komplexen Eigenschaften werden als geschachtelte DbPropertyValues Objekte zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="d205a-179">When you use CurrentValues, OriginalValues, or GetDatabaseValues to get all the current, original, or database values for an entity, the values of any complex properties are returned as nested DbPropertyValues objects.</span></span> <span data-ttu-id="d205a-180">Diese geschachtelte Objekte kann dann verwendet werden, um die Werte des komplexen Objekts zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="d205a-180">These nested objects can then be used to get values of the complex object.</span></span> <span data-ttu-id="d205a-181">Beispielsweise wird die folgende Methode, die Werte aller Eigenschaften, einschließlich der Werte von komplexen Eigenschaften und verschachtelte komplexe Eigenschaften ausgegeben.</span><span class="sxs-lookup"><span data-stu-id="d205a-181">For example, the following method will print out the values of all properties, including values of any complex properties and nested complex properties.</span></span>  

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

<span data-ttu-id="d205a-182">Um die Methode alle aktuellen Eigenschaftswerte auszudrucken würde wie folgt aufgerufen werden:</span><span class="sxs-lookup"><span data-stu-id="d205a-182">To print out all current property values the method would be called like this:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
