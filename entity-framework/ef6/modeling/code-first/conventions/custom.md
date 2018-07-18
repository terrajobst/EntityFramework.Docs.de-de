---
title: Benutzerdefinierte Code First-Konventionen - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
caps.latest.revision: 3
ms.openlocfilehash: 24d6f1bd5eb2ff8be59b9eedd1c4156709fa42fb
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2018
ms.locfileid: "39121366"
---
# <a name="custom-code-first-conventions"></a><span data-ttu-id="04e7a-102">Benutzerdefinierte Code First-Konventionen</span><span class="sxs-lookup"><span data-stu-id="04e7a-102">Custom Code First Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="04e7a-103">**Nur EF6 und höher:** Die Features, APIs usw., die auf dieser Seite erläutert werden, wurden in Entity Framework 6 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="04e7a-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="04e7a-104">Wenn Sie eine frühere Version verwenden, gelten manche Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="04e7a-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="04e7a-105">Wenn Sie Code First verwenden das Modell aus den Klassen, die mit einem Satz von Konventionen berechnet wird.</span><span class="sxs-lookup"><span data-stu-id="04e7a-105">When using Code First your model is calculated from your classes using a set of conventions.</span></span> <span data-ttu-id="04e7a-106">Der Standardwert [Code First-Konventionen](~/ef6/modeling/code-first/conventions/built-in.md) wie die Eigenschaft wird der Primärschlüssel einer Entität, die den Namen des in der Tabelle eine Entität zugeordnet und welche Genauigkeit und Dezimalstellenanzahl verfügt über Spalten im Dezimalformat standardmäßig zu ermitteln.</span><span class="sxs-lookup"><span data-stu-id="04e7a-106">The default [Code First Conventions](~/ef6/modeling/code-first/conventions/built-in.md) determine things like which property becomes the primary key of an entity, the name of the table an entity maps to, and what precision and scale a decimal column has by default.</span></span>

<span data-ttu-id="04e7a-107">Dieser Standardkonventionen sind manchmal nicht optimal geeignet für Ihr Modell, und Sie diese umgehen, indem viele einzelne Elemente, die mithilfe von Datenanmerkungen oder der Fluent-API konfigurieren müssen.</span><span class="sxs-lookup"><span data-stu-id="04e7a-107">Sometimes these default conventions are not ideal for your model, and you have to work around them by configuring many individual entities using Data Annotations or the Fluent API.</span></span> <span data-ttu-id="04e7a-108">Benutzerdefinierte Code First-Konventionen können Sie Ihre eigenen Konventionen zu definieren, die Standardeinstellungen der Konfiguration für Ihr Modell bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="04e7a-108">Custom Code First Conventions let you define your own conventions that provide configuration defaults for your model.</span></span> <span data-ttu-id="04e7a-109">In dieser exemplarischen Vorgehensweise erforschen wir die verschiedenen Typen von benutzerdefinierten Konventionen und wie Sie jeweils zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="04e7a-109">In this walkthrough, we will explore the different types of custom conventions and how to create each of them.</span></span>


## <a name="model-based-conventions"></a><span data-ttu-id="04e7a-110">Modellbasierten Konventionen</span><span class="sxs-lookup"><span data-stu-id="04e7a-110">Model-Based Conventions</span></span>

<span data-ttu-id="04e7a-111">Auf dieser Seite werden der DbModelBuilder-API für benutzerdefinierte Konventionen behandelt.</span><span class="sxs-lookup"><span data-stu-id="04e7a-111">This page covers the DbModelBuilder API for custom conventions.</span></span> <span data-ttu-id="04e7a-112">Diese API sollte für die Erstellung von den meisten benutzerdefinierter Konventionen ausreichend sein.</span><span class="sxs-lookup"><span data-stu-id="04e7a-112">This API should be sufficient for authoring most custom conventions.</span></span> <span data-ttu-id="04e7a-113">Es ist jedoch auch die Möglichkeit zum Erstellen von modellbasierten Konventionen - Konventionen, die das endgültige Modell bearbeiten, nachdem es erstellt wurde – erweiterte Szenarien behandelt.</span><span class="sxs-lookup"><span data-stu-id="04e7a-113">However, there is also the ability to author model-based conventions - conventions that manipulate the final model once it is created - to handle advanced scenarios.</span></span> <span data-ttu-id="04e7a-114">Weitere Informationen finden Sie unter [modellbasierten Konventionen](~/ef6/modeling/code-first/conventions/model.md).</span><span class="sxs-lookup"><span data-stu-id="04e7a-114">For more information, see [Model-Based Conventions](~/ef6/modeling/code-first/conventions/model.md).</span></span>

 

## <a name="our-model"></a><span data-ttu-id="04e7a-115">Unser Modell</span><span class="sxs-lookup"><span data-stu-id="04e7a-115">Our Model</span></span>

<span data-ttu-id="04e7a-116">Zuerst definieren wir ein einfaches Modell, das wir mit unserer Konventionen verwenden können.</span><span class="sxs-lookup"><span data-stu-id="04e7a-116">Let's start by defining a simple model that we can use with our conventions.</span></span> <span data-ttu-id="04e7a-117">Fügen Sie Ihrem Projekt die folgenden Klassen hinzu.</span><span class="sxs-lookup"><span data-stu-id="04e7a-117">Add the following classes to your project.</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;

    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }
    }

    public class Product
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public decimal? Price { get; set; }
        public DateTime? ReleaseDate { get; set; }
        public ProductCategory Category { get; set; }
    }

    public class ProductCategory
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public List<Product> Products { get; set; }
    }
```

 

## <a name="introducing-custom-conventions"></a><span data-ttu-id="04e7a-118">Einführung in benutzerdefinierte Konventionen</span><span class="sxs-lookup"><span data-stu-id="04e7a-118">Introducing Custom Conventions</span></span>

<span data-ttu-id="04e7a-119">Schreiben wir nun eine Konvention, die eine Eigenschaft mit dem Namen Taste, um den primären Schlüssel für den Entitätstyp werden konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="04e7a-119">Let’s write a convention that configures any property named Key to be the primary key for its entity type.</span></span>

<span data-ttu-id="04e7a-120">Konventionen sind auf den Modell-Generator aktiviert, die durch das Überschreiben von "onmodelcreating" im Kontext zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="04e7a-120">Conventions are enabled on the model builder, which can be accessed by overriding OnModelCreating in the context.</span></span> <span data-ttu-id="04e7a-121">Aktualisieren Sie die ProductContext-Klasse wie folgt:</span><span class="sxs-lookup"><span data-stu-id="04e7a-121">Update the ProductContext class as follows:</span></span>

``` csharp
    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Properties()
                        .Where(p => p.Name == "Key")
                        .Configure(p => p.IsKey());
        }
    }
```

<span data-ttu-id="04e7a-122">Jetzt wird jede Eigenschaft in unserem Modell benannten Schlüssel konfiguriert als primären Schlüssel für beliebige Entität seinen Teil.</span><span class="sxs-lookup"><span data-stu-id="04e7a-122">Now, any property in our model named Key will be configured as the primary key of whatever entity its part of.</span></span>

<span data-ttu-id="04e7a-123">Wir konnten stellen Sie außerdem unsere Konventionen spezifischere durch den Typ der Eigenschaft, die an das Konfigurieren von Filtern:</span><span class="sxs-lookup"><span data-stu-id="04e7a-123">We could also make our conventions more specific by filtering on the type of property that we are going to configure:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

<span data-ttu-id="04e7a-124">Dadurch wird konfiguriert, dass alle Eigenschaften, die aufgerufen werden Schlüssel, um den primären Schlüssel, der die Entität, jedoch nur, wenn sie eine ganze Zahl sind.</span><span class="sxs-lookup"><span data-stu-id="04e7a-124">This will configure all properties called Key to be the primary key of their entity, but only if they are an integer.</span></span>

<span data-ttu-id="04e7a-125">Ein interessantes Feature der IsKey-Methode besteht darin, dass additiv.</span><span class="sxs-lookup"><span data-stu-id="04e7a-125">An interesting feature of the IsKey method is that it is additive.</span></span> <span data-ttu-id="04e7a-126">Das bedeutet, dass, wenn Sie mehrere Eigenschaften IsKey aufrufen und alle Teil eines zusammengesetzten Schlüssels werden.</span><span class="sxs-lookup"><span data-stu-id="04e7a-126">Which means that if you call IsKey on multiple properties and they will all become part of a composite key.</span></span> <span data-ttu-id="04e7a-127">Ein Nachteil dafür ist, dass wenn Sie mehrere Eigenschaften für einen Schlüssel angeben, auch einen Auftrag für diese Eigenschaften angeben müssen.</span><span class="sxs-lookup"><span data-stu-id="04e7a-127">The one caveat for this is that when you specify multiple properties for a key you must also specify an order for those properties.</span></span> <span data-ttu-id="04e7a-128">Dazu können Sie die Methode wie die folgende HasColumnOrder aufrufen:</span><span class="sxs-lookup"><span data-stu-id="04e7a-128">You can do this by calling the HasColumnOrder method like below:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

<span data-ttu-id="04e7a-129">Dieser Code konfiguriert die Typen in unserem Modell haben Sie einen zusammengesetzten Schlüssel, der aus die Int-Key-Spalte und die Namensspalte Zeichenfolge besteht.</span><span class="sxs-lookup"><span data-stu-id="04e7a-129">This code will configure the types in our model to have a composite key consisting of the int Key column and the string Name column.</span></span> <span data-ttu-id="04e7a-130">Wenn wir das Modell im Designer anzuzeigen, würde es folgendermaßen aussehen:</span><span class="sxs-lookup"><span data-stu-id="04e7a-130">If we view the model in the designer it would look like this:</span></span>

![compositeKey](~/ef6/media/compositekey.png)

<span data-ttu-id="04e7a-132">Ein weiteres Beispiel für Eigenschaft Konventionen ist so konfigurieren Sie alle Eigenschaften von "DateTime" in meinem Modell in den datetime2-Typ in SQL Server anstelle von "DateTime" zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="04e7a-132">Another example of property conventions is to configure all DateTime properties in my model to map to the datetime2 type in SQL Server instead of datetime.</span></span> <span data-ttu-id="04e7a-133">Sie erreichen dies durch den folgenden:</span><span class="sxs-lookup"><span data-stu-id="04e7a-133">You can achieve this with the following:</span></span>

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a><span data-ttu-id="04e7a-134">Konvention-Klassen</span><span class="sxs-lookup"><span data-stu-id="04e7a-134">Convention Classes</span></span>

<span data-ttu-id="04e7a-135">Eine weitere Möglichkeit zum Definieren von Konventionen ist eine Konvention-Klasse verwendet, um Ihre Konvention zu kapseln.</span><span class="sxs-lookup"><span data-stu-id="04e7a-135">Another way of defining conventions is to use a Convention Class to encapsulate your convention.</span></span> <span data-ttu-id="04e7a-136">Bei Verwendung einer Konvention-Klasse erstellen Sie einen Typ, der von der Konvention-Klasse im Namespace System.Data.Entity.ModelConfiguration.Conventions erbt.</span><span class="sxs-lookup"><span data-stu-id="04e7a-136">When using a Convention Class then you create a type that inherits from the Convention class in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>

<span data-ttu-id="04e7a-137">Wir können eine Konvention-Klasse mit der datetime2-Konvention erstellen, die wir weiter oben gezeigt habe, wie folgt:</span><span class="sxs-lookup"><span data-stu-id="04e7a-137">We can create a Convention Class with the datetime2 convention that we showed earlier by doing the following:</span></span>

``` csharp
    public class DateTime2Convention : Convention
    {
        public DateTime2Convention()
        {
            this.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));        
        }
    }
```

<span data-ttu-id="04e7a-138">Informieren von EF diese Konvention verwenden Sie es der Konventionen-Auflistung in "OnModelCreating" hinzufügen, wenn Sie zusammen mit der exemplarischen Vorgehensweise durchgearbeitet haben so aussieht:</span><span class="sxs-lookup"><span data-stu-id="04e7a-138">To tell EF to use this convention you add it to the Conventions collection in OnModelCreating, which if you’ve been following along with the walkthrough will look like this:</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

<span data-ttu-id="04e7a-139">Wie Sie, dass wir eine Instanz von unserem Konvention der Konventionen Auflistung hinzufügen sehen können.</span><span class="sxs-lookup"><span data-stu-id="04e7a-139">As you can see we add an instance of our convention to the conventions collection.</span></span> <span data-ttu-id="04e7a-140">Erben von Konvention bietet eine bequeme Möglichkeit, einer Gruppierung und Konventionen für Teams oder Projekte freigegeben.</span><span class="sxs-lookup"><span data-stu-id="04e7a-140">Inheriting from Convention provides a convenient way of grouping and sharing conventions across teams or projects.</span></span> <span data-ttu-id="04e7a-141">Sie könnten z. B. eine Klassenbibliothek mit einem gemeinsamen Satz von Konventionen, dass alle von Ihrer Organisation Projekte haben.</span><span class="sxs-lookup"><span data-stu-id="04e7a-141">You could, for example, have a class library with a common set of conventions that all of your organizations projects use.</span></span>

 

## <a name="custom-attributes"></a><span data-ttu-id="04e7a-142">Benutzerdefinierte Attribute</span><span class="sxs-lookup"><span data-stu-id="04e7a-142">Custom Attributes</span></span>

<span data-ttu-id="04e7a-143">Weiteres können Sie Konventionen ist so aktivieren Sie neue Attribute verwendet werden, wenn Sie ein Modell zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="04e7a-143">Another great use of conventions is to enable new attributes to be used when configuring a model.</span></span> <span data-ttu-id="04e7a-144">Um dies zu veranschaulichen, wir erstellen ein Attribut, das wir zum Markieren von Zeichenfolgeneigenschaften als nicht-Unicode verwenden können.</span><span class="sxs-lookup"><span data-stu-id="04e7a-144">To illustrate this, let’s create an attribute that we can use to mark String properties as non-Unicode.</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

<span data-ttu-id="04e7a-145">Nun erstellen wir eine Konvention, um dieses Attribut auf unserem Modell anwenden:</span><span class="sxs-lookup"><span data-stu-id="04e7a-145">Now, let’s create a convention to apply this attribute to our model:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

<span data-ttu-id="04e7a-146">Diese Konvention können wir hinzufügen, dass das nicht-Unicode-Attribut auf unsere Zeichenfolgeneigenschaften, was bedeutet die Spalte in der Datenbank als Varchar, anstatt Nvarchar gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="04e7a-146">With this convention we can add the NonUnicode attribute to any of our string properties, which means the column in the database will be stored as varchar instead of nvarchar.</span></span>

<span data-ttu-id="04e7a-147">Informationen zu dieser Konvention zu beachten ist, die, wenn Sie das nicht-Unicode-Attribut für etwas anderes als eine String-Eigenschaft festlegen, und klicken Sie dann eine Ausnahme ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="04e7a-147">One thing to note about this convention is that if you put the NonUnicode attribute on anything other than a string property then it will throw an exception.</span></span> <span data-ttu-id="04e7a-148">Es liegt daran, dass Sie nicht auf einem anderen Typ als eine Zeichenfolge IsUnicode konfigurieren können.</span><span class="sxs-lookup"><span data-stu-id="04e7a-148">It does this because you cannot configure IsUnicode on any type other than a string.</span></span> <span data-ttu-id="04e7a-149">In diesem Fall können Sie Ihre Konvention genauer gesagt ausführen, damit es etwas herausfiltert, die nicht von einer Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="04e7a-149">If this happens, then you can make your convention more specific, so that it filters out anything that isn’t a string.</span></span>

<span data-ttu-id="04e7a-150">Die oben genannten Konvention zum Definieren von benutzerdefinierter Attributs funktioniert wird es eine andere API, die viel einfacher zu verwenden, insbesondere wenn Sie möchten die Eigenschaften der Attributklasse verwenden werden können.</span><span class="sxs-lookup"><span data-stu-id="04e7a-150">While the above convention works for defining custom attributes there is another API that can be much easier to use, especially when you want to use properties from the attribute class.</span></span>

<span data-ttu-id="04e7a-151">In diesem Beispiel werden wir unseren Attribut aktualisieren und ändern Sie ihn in ein IsUnicode-Attribut, sodass sie wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="04e7a-151">For this example we are going to update our attribute and change it to an IsUnicode attribute, so it looks like this:</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    internal class IsUnicode : Attribute
    {
        public bool Unicode { get; set; }

        public IsUnicode(bool isUnicode)
        {
            Unicode = isUnicode;
        }
    }
```

<span data-ttu-id="04e7a-152">Sobald wir dies haben, können wir einen booleschen Wert festlegen, auf unsere Attribut der Konvention mitteilen, ob eine Eigenschaft Unicode sein sollte.</span><span class="sxs-lookup"><span data-stu-id="04e7a-152">Once we have this, we can set a bool on our attribute to tell the convention whether or not a property should be Unicode.</span></span> <span data-ttu-id="04e7a-153">Wir könnten dies in der Konvention haben wir bereits durch den Zugriff auf die ClrProperty der Configuration-Klasse wie folgt vorgehen:</span><span class="sxs-lookup"><span data-stu-id="04e7a-153">We could do this in the convention we have already by accessing the ClrProperty of the configuration class like this:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

<span data-ttu-id="04e7a-154">Dies ist recht einfach, aber es ist eine kompaktere Möglichkeit zum erreichen Sie dies mithilfe der Having-Methode der den API-Konventionen.</span><span class="sxs-lookup"><span data-stu-id="04e7a-154">This is easy enough, but there is a more succinct way of achieving this by using the Having method of the conventions API.</span></span> <span data-ttu-id="04e7a-155">Die Having Methode hat einen Parameter vom Typ Func&lt;PropertyInfo, T&gt; akzeptiert der PropertyInfo identisch mit der Where-Methode, aber erwartet wird, um ein Objekt zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="04e7a-155">The Having method has a parameter of type Func&lt;PropertyInfo, T&gt; which accepts the PropertyInfo the same as the Where method, but is expected to return an object.</span></span> <span data-ttu-id="04e7a-156">Wenn das zurückgegebene Objekt null ist, und klicken Sie dann die Eigenschaft nicht konfiguriert werden soll, das bedeutet, dass Eigenschaften, die mit ihm wie Where filtern kann, aber es unterscheidet sich auch das zurückgegebene Objekt zu erfassen und an die Configure-Methode übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="04e7a-156">If the returned object is null then the property will not be configured, which means you can filter out properties with it just like Where, but it is different in that it will also capture the returned object and pass it to the Configure method.</span></span> <span data-ttu-id="04e7a-157">Dies funktioniert folgendermaßen:</span><span class="sxs-lookup"><span data-stu-id="04e7a-157">This works like the following:</span></span>

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

<span data-ttu-id="04e7a-158">Benutzerdefinierte Attribute sind nicht der einzige Grund für das Verwenden der Having-Methode, es ist nützlich, überall, die etwas befassen müssen, die Sie beim Konfigurieren Ihrer Typen oder Eigenschaften filtern möchten.</span><span class="sxs-lookup"><span data-stu-id="04e7a-158">Custom attributes are not the only reason to use the Having method, it is useful anywhere that you need to reason about something that you are filtering on when configuring your types or properties.</span></span>

 

## <a name="configuring-types"></a><span data-ttu-id="04e7a-159">Konfigurieren von Typen</span><span class="sxs-lookup"><span data-stu-id="04e7a-159">Configuring Types</span></span>

<span data-ttu-id="04e7a-160">Bisher wurden aller unserer Konventionen für Eigenschaften, aber einen anderen Bereich der Konventionen API besteht, für die Konfiguration der Typen im Modell.</span><span class="sxs-lookup"><span data-stu-id="04e7a-160">So far all of our conventions have been for properties, but there is another area of the conventions API for configuring the types in your model.</span></span> <span data-ttu-id="04e7a-161">Die Benutzeroberfläche ähnelt der Konventionen, die wir eben gesehen haben, aber die Optionen innerhalb konfigurieren, findet sich auf der Entität anstatt nach Eigenschaft auf.</span><span class="sxs-lookup"><span data-stu-id="04e7a-161">The experience is similar to the conventions we have seen so far, but the options inside configure will be at the entity instead of property level.</span></span>

<span data-ttu-id="04e7a-162">Eines der Dinge, denen Ebene für Parametertypen besonders nützlich sein können wird die Tabelle-Namenskonvention gilt, um ein vorhandenes Schema zuzuordnen, die von den EF-Standard abweicht oder zum Erstellen einer neuen Datenbank mit einer anderen Namenskonvention geändert.</span><span class="sxs-lookup"><span data-stu-id="04e7a-162">One of the things that Type level conventions can be really useful for is changing the table naming convention, either to map to an existing schema that differs from the EF default or to create a new database with a different naming convention.</span></span> <span data-ttu-id="04e7a-163">Zu diesem Zweck benötigen wir zuerst eine Methode, die akzeptieren die TypeInfo für einen Typ in unserem Modell kann, und zurückgeben, was der Tabellennamen für diesen Typ werden soll:</span><span class="sxs-lookup"><span data-stu-id="04e7a-163">To do this we first need a method that can accept the TypeInfo for a type in our model and return what the table name for that type should be:</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

<span data-ttu-id="04e7a-164">Diese Methode akzeptiert einen Typ und gibt eine Zeichenfolge, die Kleinbuchstaben mit Unterstrichen anstelle von CamelCase verwendet.</span><span class="sxs-lookup"><span data-stu-id="04e7a-164">This method takes a type and returns a string that uses lower case with underscores instead of CamelCase.</span></span> <span data-ttu-id="04e7a-165">In unserem Modell bedeutet dies, dass die ProductCategory-Klasse, eine Tabelle namens Produkt zugeordnet werden soll\_Kategorie anstelle von ProductCategories.</span><span class="sxs-lookup"><span data-stu-id="04e7a-165">In our model this means that the ProductCategory class will be mapped to a table called product\_category instead of ProductCategories.</span></span>

<span data-ttu-id="04e7a-166">Nachdem wir diese Methode erstellt haben, können wir es in eine Konvention wie folgt aufrufen:</span><span class="sxs-lookup"><span data-stu-id="04e7a-166">Once we have that method we can call it in a convention like this:</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

<span data-ttu-id="04e7a-167">Diese Konvention konfiguriert jeden Typ in unserem Modell auf den Namen der Tabelle zugeordnet, die von unseren GetTableName-Methode zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="04e7a-167">This convention configures every type in our model to map to the table name that is returned from our GetTableName method.</span></span> <span data-ttu-id="04e7a-168">Diese Konvention entspricht dem Aufruf der ToTable-Methode für jede Entität im Modell mithilfe der Fluent-API.</span><span class="sxs-lookup"><span data-stu-id="04e7a-168">This convention is the equivalent to calling the ToTable method for each entity in the model using the Fluent API.</span></span>

<span data-ttu-id="04e7a-169">Informationen hierzu zu beachten ist aus, dass beim Aufrufen ToTable EF die Zeichenfolge, die Sie als genaue Tabellennamen, ohne die Pluralisierung bereitstellen, die sie normalerweise, beim Tabellennamen zu bestimmen ausführen würden.</span><span class="sxs-lookup"><span data-stu-id="04e7a-169">One thing to note about this is that when you call ToTable EF will take the string that you provide as the exact table name, without any of the pluralization that it would normally do when determining table names.</span></span> <span data-ttu-id="04e7a-170">Daher ist der Tabellenname aus unserer Konvention Produkt\_Kategorie anstelle von Produkt\_Kategorien.</span><span class="sxs-lookup"><span data-stu-id="04e7a-170">This is why the table name from our convention is product\_category instead of product\_categories.</span></span> <span data-ttu-id="04e7a-171">Wir können, die in unserer Konvention Anruf an den pluralisierungsdienst selbst beheben.</span><span class="sxs-lookup"><span data-stu-id="04e7a-171">We can resolve that in our convention by making a call to the pluralization service ourselves.</span></span>

<span data-ttu-id="04e7a-172">In den folgenden Code verwenden wir die [Abhängigkeitsauflösung](~/ef6/fundamentals/configuring/dependency-resolution.md) Feature hinzugefügt, die in EF6 die Pluralisierung abrufen, die EF verwendet haben, wird in den Singular unsere Tabellenname und.</span><span class="sxs-lookup"><span data-stu-id="04e7a-172">In the following code we will use the [Dependency Resolution](~/ef6/fundamentals/configuring/dependency-resolution.md) feature added in EF6 to retrieve the pluralization service that EF would have used and pluralize our table name.</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var pluralizationService = DbConfiguration.DependencyResolver.GetService<IPluralizationService>();

        var result = pluralizationService.Pluralize(type.Name);

        result = Regex.Replace(result, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

> [!NOTE]
> <span data-ttu-id="04e7a-173">Die generische Version von "GetService" ist eine Erweiterungsmethode im System.Data.Entity.Infrastructure.DependencyResolution-Namespace, müssen Sie zum Hinzufügen einer using-Anweisung den Kontext, um sie zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="04e7a-173">The generic version of GetService is an extension method in the System.Data.Entity.Infrastructure.DependencyResolution namespace, you will need to add a using statement to your context in order to use it.</span></span>

### <a name="totable-and-inheritance"></a><span data-ttu-id="04e7a-174">ToTable und Vererbung</span><span class="sxs-lookup"><span data-stu-id="04e7a-174">ToTable and Inheritance</span></span>

<span data-ttu-id="04e7a-175">Ein weiterer wichtiger Aspekt der ToTable ist, sofern explizit eines Typs für eine bestimmte Tabelle zuordnen, und klicken Sie dann Sie die Zuordnungsstrategie für die ändern können, die EF verwenden.</span><span class="sxs-lookup"><span data-stu-id="04e7a-175">Another important aspect of ToTable is that if you explicitly map a type to a given table, then you can alter the mapping strategy that EF will use.</span></span> <span data-ttu-id="04e7a-176">Wenn Sie für jeden Typ in einer Vererbungshierarchie ToTable aufrufen, werden der Typname als den Namen der Tabelle übergeben wird, wie oben, Sie Zuordnung die Standardstrategie-Tabelle pro Hierarchie (TPH), Tabelle pro Typ (TPT) ändern.</span><span class="sxs-lookup"><span data-stu-id="04e7a-176">If you call ToTable for every type in an inheritance hierarchy, passing the type name as the name of the table like we did above, then you will change the default Table-Per-Hierarchy (TPH) mapping strategy to Table-Per-Type (TPT).</span></span> <span data-ttu-id="04e7a-177">Die beste Möglichkeit, dies ist innerhalb eines konkreten Beispiels:</span><span class="sxs-lookup"><span data-stu-id="04e7a-177">The best way to describe this is whith a concrete example:</span></span>

``` csharp
    public class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Manager : Employee
    {
        public string SectionManaged { get; set; }
    }
```

<span data-ttu-id="04e7a-178">Standardmäßig sind sowohl Mitarbeiter und Manager, die der gleichen Tabelle (Mitarbeiter) in der Datenbank zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="04e7a-178">By default both employee and manager are mapped to the same table (Employees) in the database.</span></span> <span data-ttu-id="04e7a-179">Die Tabelle enthält, Mitarbeitern und Managern mit der eine Unterscheidungsspalte, die Sie darüber informiert, welche Art von Instanz, die in jeder Zeile gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="04e7a-179">The table will contain both employees and managers with a discriminator column that will tell you what type of instance is stored in each row.</span></span> <span data-ttu-id="04e7a-180">Dies ist eine TPH-Zuordnung, wie eine einzelne Tabelle für die Hierarchie vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="04e7a-180">This is TPH mapping as there is a single table for the hierarchy.</span></span> <span data-ttu-id="04e7a-181">Allerdings, wenn Sie für beide Classe ToTable aufrufen, wird Klicken Sie dann jeden Typ stattdessen zugeordnet werden, auch bekannt als TPT, da jeder eine eigene Tabelle verfügt über eine eigene Tabelle.</span><span class="sxs-lookup"><span data-stu-id="04e7a-181">However, if you call ToTable on both classe then each type will instead be mapped to its own table, also known as TPT since each type has its own table.</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

<span data-ttu-id="04e7a-182">Der obige Code wird die Struktur einer Tabelle zugeordnet sein, die wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="04e7a-182">The code above will map to a table structure that looks like the following:</span></span>

![tptExample](~/ef6/media/tptexample.jpg)

<span data-ttu-id="04e7a-184">Sie können dies verhindern, und die standardmäßig TPH-Zuordnung, auf unterschiedliche Weise verwalten:</span><span class="sxs-lookup"><span data-stu-id="04e7a-184">You can avoid this, and maintain the default TPH mapping, in a couple ways:</span></span>

1.  <span data-ttu-id="04e7a-185">Rufen Sie ToTable, mit dem gleichen Tabellennamen für jeden Typ in der Hierarchie.</span><span class="sxs-lookup"><span data-stu-id="04e7a-185">Call ToTable with the same table name for each type in the hierarchy.</span></span>
2.  <span data-ttu-id="04e7a-186">Rufen Sie ToTable nur für die Basisklasse der Hierarchie, in unserem Beispiel, die Mitarbeiter sind.</span><span class="sxs-lookup"><span data-stu-id="04e7a-186">Call ToTable only on the base class of the hierarchy, in our example that would be employee.</span></span>

 

## <a name="execution-order"></a><span data-ttu-id="04e7a-187">Ausführungsreihenfolge</span><span class="sxs-lookup"><span data-stu-id="04e7a-187">Execution Order</span></span>

<span data-ttu-id="04e7a-188">Konventionen, die in letzten Wins Weise identisch mit der Fluent-API ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="04e7a-188">Conventions operate in a last wins manner, the same as the Fluent API.</span></span> <span data-ttu-id="04e7a-189">Das bedeutet, ist, wenn Sie zwei Konventionen, die die gleiche Möglichkeit, die gleiche Eigenschaft konfigurieren, und klicken Sie dann auf die letzte Lektion zum Ausführen von Wins schreiben.</span><span class="sxs-lookup"><span data-stu-id="04e7a-189">What this means is that if you write two conventions that configure the same option of the same property, then the last one to execute wins.</span></span> <span data-ttu-id="04e7a-190">Beispielsweise können in den folgenden Code wird die maximale Länge aller Zeichenfolgen auf 500 festgelegt, aber wir konfigurieren Sie dann alle Eigenschaften, die mit dem Namen Name im Modell zu eine maximale Länge von 250 haben.</span><span class="sxs-lookup"><span data-stu-id="04e7a-190">As an example, in the code below the max length of all strings is set to 500 but we then configure all properties called Name in the model to have a max length of 250.</span></span>

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

<span data-ttu-id="04e7a-191">Da Konvention, die die maximalen Länge auf 250 festgelegt nach der ist, die alle Zeichenfolgen auf 500 festgelegt, werden alle Eigenschaften, die mit dem Namen Name in unserem Modell weist eine maximale Länge 250, während andere Zeichenfolgen, z. B. Beschreibungen, wäre 500.</span><span class="sxs-lookup"><span data-stu-id="04e7a-191">Because the convention to set max length to 250 is after the one that sets all strings to 500, all the properties called Name in our model will have a MaxLength of 250 while any other strings, such as descriptions, would be 500.</span></span> <span data-ttu-id="04e7a-192">Mithilfe von Konventionen auf diese Weise bedeutet, dass Sie eine allgemeine Konvention für Typen oder Eigenschaften in Ihr Modell und erstellt dann einen override aufgenommen bereitstellen, können Sie Teilmengen, die unterschiedlich sind.</span><span class="sxs-lookup"><span data-stu-id="04e7a-192">Using conventions in this way means that you can provide a general convention for types or properties in your model and then overide them for subsets that are different.</span></span>

<span data-ttu-id="04e7a-193">Die Fluent-API und die Datenanmerkungen kann auch verwendet werden, um eine Konvention in bestimmten Fällen zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="04e7a-193">The Fluent API and Data Annotations can also be used to override a convention in specific cases.</span></span> <span data-ttu-id="04e7a-194">Im Beispiel oben hätten wir die Fluent-API verwendet, legen Sie die maximale Länge einer Eigenschaft konnte klicken Sie dann wir es vor oder nach der Konvention eingefügt haben, da die spezifischere Fluent-API über die allgemeine Konfiguration Konvention gewinnen wird.</span><span class="sxs-lookup"><span data-stu-id="04e7a-194">In our example above if we had used the Fluent API to set the max length of a property then we could have put it before or after the convention, because the more specific Fluent API will win over the more general Configuration Convention.</span></span>

 

## <a name="built-in-conventions"></a><span data-ttu-id="04e7a-195">Integrierte Konventionen</span><span class="sxs-lookup"><span data-stu-id="04e7a-195">Built-in Conventions</span></span>

<span data-ttu-id="04e7a-196">Da benutzerdefinierte Konventionen von den Standardkonventionen für Code First auswirken könnten, kann es nützlich, um das Hinzufügen von Konventionen, die vor oder nach einem anderen Konvention ausgeführt sein.</span><span class="sxs-lookup"><span data-stu-id="04e7a-196">Because custom conventions could be affected by the default Code First conventions, it can be useful to add conventions to run before or after another convention.</span></span> <span data-ttu-id="04e7a-197">Zu diesem Zweck können Sie die Methoden "AddBefore" und AddAfter des der Auflistung der Konventionen in Ihrer abgeleiteten DbContext.</span><span class="sxs-lookup"><span data-stu-id="04e7a-197">To do this you can use the AddBefore and AddAfter methods of the Conventions collection on your derived DbContext.</span></span> <span data-ttu-id="04e7a-198">Der folgende Code würde fügen Sie die Konvention-Klasse, die wir zuvor erstellt haben, damit er vor den integrierten ausgeführt wird in der Ermittlung des Konvention.</span><span class="sxs-lookup"><span data-stu-id="04e7a-198">The following code would add the convention class we created earlier so that it will run before the built in key discovery convention.</span></span>

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

<span data-ttu-id="04e7a-199">Hier werden die meisten nützlich sein, wenn Konventionen hinzufügen, die vor oder nach der integrierten Konventionen ausführen müssen, eine Liste der integrierten Konventionen finden Sie hier: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx) .</span><span class="sxs-lookup"><span data-stu-id="04e7a-199">This is going to be of the most use when adding conventions that need to run before or after the built in conventions, a list of the built in conventions can be found here: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>

<span data-ttu-id="04e7a-200">Sie können auch entfernen Konventionen, die Sie nicht möchten, auf das Modell angewendet.</span><span class="sxs-lookup"><span data-stu-id="04e7a-200">You can also remove conventions that you do not want applied to your model.</span></span> <span data-ttu-id="04e7a-201">Um eine Konvention zu entfernen, verwenden Sie die Remove-Methode.</span><span class="sxs-lookup"><span data-stu-id="04e7a-201">To remove a convention, use the Remove method.</span></span> <span data-ttu-id="04e7a-202">Hier ist ein Beispiel für die PluralizingTableNameConvention entfernen.</span><span class="sxs-lookup"><span data-stu-id="04e7a-202">Here is an example of removing the PluralizingTableNameConvention.</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
