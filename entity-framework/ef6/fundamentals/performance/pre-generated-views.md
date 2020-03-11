---
title: Vorab generierte Mapping-Sichten-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
ms.openlocfilehash: 1fda9fe9638adce9b24a6b81aa081effeb0def81
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416030"
---
# <a name="pre-generated-mapping-views"></a><span data-ttu-id="f98ed-102">Vorab generierte Mapping-Sichten</span><span class="sxs-lookup"><span data-stu-id="f98ed-102">Pre-generated mapping views</span></span>
<span data-ttu-id="f98ed-103">Bevor die Entity Framework eine Abfrage ausführen oder Änderungen an der Datenquelle speichern kann, muss Sie einen Satz von zuordnungssichten generieren, um auf die Datenbank zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="f98ed-103">Before the Entity Framework can execute a query or save changes to the data source, it must generate a set of mapping views to access the database.</span></span> <span data-ttu-id="f98ed-104">Diese Zuweisungs Sichten sind eine Reihe von Entity SQL-Anweisungen, die die Datenbank auf abstrakte Weise darstellen. Sie sind Teil der Metadaten, die pro Anwendungsdomäne zwischengespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="f98ed-104">These mapping views are a set of Entity SQL statement that represent the database in an abstract way, and are part of the metadata which is cached per application domain.</span></span> <span data-ttu-id="f98ed-105">Wenn Sie mehrere Instanzen desselben Kontexts in derselben Anwendungsdomäne erstellen, werden die Zuordnung von Sichten aus den zwischengespeicherten Metadaten wieder verwendet, anstatt Sie neu zu generieren.</span><span class="sxs-lookup"><span data-stu-id="f98ed-105">If you create multiple instances of the same context in the same application domain, they will reuse mapping views from the cached metadata rather than regenerating them.</span></span> <span data-ttu-id="f98ed-106">Da die Generierung der Ansichts Generierung ein wesentlicher Bestandteil der Gesamtkosten für die Ausführung der ersten Abfrage ist, ermöglicht Ihnen die Entity Framework, die Zuordnung von Sichten vorab zu generieren und Sie in das kompilierte Projekt einzubeziehen.</span><span class="sxs-lookup"><span data-stu-id="f98ed-106">Because mapping view generation is a significant part of the overall cost of executing the first query, the Entity Framework enables you to pre-generate mapping views and include them in the compiled project.</span></span><span data-ttu-id="f98ed-107"> Weitere Informationen finden Sie unter  [Überlegungen zur Leistung (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span><span class="sxs-lookup"><span data-stu-id="f98ed-107"> For more information, see  [Performance Considerations (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span></span>

## <a name="generating-mapping-views-with-the-ef-power-tools-community-edition"></a><span data-ttu-id="f98ed-108">Erstellen von zuordnungssichten mit der Entity Edition von EF Power Tools</span><span class="sxs-lookup"><span data-stu-id="f98ed-108">Generating Mapping Views with the EF Power Tools Community Edition</span></span>

<span data-ttu-id="f98ed-109">Die einfachste Möglichkeit zum vorab Generieren von Ansichten ist die Verwendung der [EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition).</span><span class="sxs-lookup"><span data-stu-id="f98ed-109">The easiest way to pre-generate views is to use the [EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition).</span></span> <span data-ttu-id="f98ed-110">Nachdem Sie die Power Tools installiert haben, verfügen Sie über eine Menüoption zum Generieren von Ansichten, wie unten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="f98ed-110">Once you have the Power Tools installed you will have a menu option to Generate Views, as below.</span></span>

-   <span data-ttu-id="f98ed-111">Klicken Sie für **Code First** Modelle mit der rechten Maustaste auf die Codedatei, in der die dbcontext-Klasse enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="f98ed-111">For **Code First** models right-click on the code file that contains your DbContext class.</span></span>
-   <span data-ttu-id="f98ed-112">Klicken Sie bei **EF-Designer** -Modellen mit der rechten Maustaste auf die EDMX-Datei.</span><span class="sxs-lookup"><span data-stu-id="f98ed-112">For **EF Designer** models right-click on your EDMX file.</span></span>

![Generieren von Sichten](~/ef6/media/generateviews.png)

<span data-ttu-id="f98ed-114">Nachdem der Vorgang abgeschlossen ist, verfügen Sie über eine Klasse, die der folgenden generierten ähnelt:</span><span class="sxs-lookup"><span data-stu-id="f98ed-114">Once the process is finished you will have a class similar to the following generated</span></span>

![generierte Sichten](~/ef6/media/generatedviews.png)

<span data-ttu-id="f98ed-116">Wenn Sie nun die Anwendung ausführen, verwendet EF diese Klasse, um Ansichten nach Bedarf zu laden.</span><span class="sxs-lookup"><span data-stu-id="f98ed-116">Now when you run your application EF will use this class to load views as required.</span></span> <span data-ttu-id="f98ed-117">Wenn sich das Modell ändert und Sie diese Klasse nicht erneut generieren, löst EF eine Ausnahme aus.</span><span class="sxs-lookup"><span data-stu-id="f98ed-117">If your model changes and you do not re-generate this class then EF will throw an exception.</span></span>

## <a name="generating-mapping-views-from-code---ef6-onwards"></a><span data-ttu-id="f98ed-118">Erstellen von Mapping-Sichten aus Code EF6 ab</span><span class="sxs-lookup"><span data-stu-id="f98ed-118">Generating Mapping Views from Code - EF6 Onwards</span></span>

<span data-ttu-id="f98ed-119">Die andere Möglichkeit zum Generieren von Sichten besteht darin, die von EF bereitgestellten APIs zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="f98ed-119">The other way to generate views is to use the APIs that EF provides.</span></span> <span data-ttu-id="f98ed-120">Wenn Sie diese Methode verwenden, haben Sie die Möglichkeit, die Ansichten beliebig zu serialisieren. Sie müssen die Ansichten jedoch auch selbst laden.</span><span class="sxs-lookup"><span data-stu-id="f98ed-120">When using this method you have the freedom to serialize the views however you like, but you also need to load the views yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="f98ed-121">**Nur EF6** : die in diesem Abschnitt gezeigten APIs wurden in Entity Framework 6 eingeführt.</span><span class="sxs-lookup"><span data-stu-id="f98ed-121">**EF6 Onwards Only** - The APIs shown in this section were introduced in Entity Framework 6.</span></span> <span data-ttu-id="f98ed-122">Wenn Sie eine frühere Version verwenden, gelten diese Informationen nicht.</span><span class="sxs-lookup"><span data-stu-id="f98ed-122">If you are using an earlier version this information does not apply.</span></span>

### <a name="generating-views"></a><span data-ttu-id="f98ed-123">Generieren von Ansichten</span><span class="sxs-lookup"><span data-stu-id="f98ed-123">Generating Views</span></span>

<span data-ttu-id="f98ed-124">Die APIs zum Generieren von Sichten befinden sich in der System. Data. Entity. Core. Mapping. StorageMappingItemCollection-Klasse.</span><span class="sxs-lookup"><span data-stu-id="f98ed-124">The APIs to generate views are on the System.Data.Entity.Core.Mapping.StorageMappingItemCollection class.</span></span> <span data-ttu-id="f98ed-125">Sie können eine storagemappingcollection für einen Kontext abrufen, indem Sie die MetadataWorkspace eines ObjectContext verwenden.</span><span class="sxs-lookup"><span data-stu-id="f98ed-125">You can retrieve a StorageMappingCollection for a Context by using the MetadataWorkspace of an ObjectContext.</span></span> <span data-ttu-id="f98ed-126">Wenn Sie die neuere dbcontext-API verwenden, können Sie mit dem iobjectcontextadapter wie unten darauf zugreifen. in diesem Code verfügen wir über eine Instanz des abgeleiteten dbcontext namens dbcontext:</span><span class="sxs-lookup"><span data-stu-id="f98ed-126">If you are using the newer DbContext API then you can access this by using the IObjectContextAdapter like below, in this code we have an instance of your derived DbContext called dbContext:</span></span>

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

<span data-ttu-id="f98ed-127">Sobald Sie über die StorageMappingItemCollection verfügen, können Sie Zugriff auf die Methoden GenerateViews und computemappinghashvalue erhalten.</span><span class="sxs-lookup"><span data-stu-id="f98ed-127">Once you have the StorageMappingItemCollection then you can get access to the GenerateViews and ComputeMappingHashValue methods.</span></span>

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

<span data-ttu-id="f98ed-128">Die erste Methode erstellt ein Wörterbuch mit einem Eintrag für jede Ansicht in der Container Zuordnung.</span><span class="sxs-lookup"><span data-stu-id="f98ed-128">The first method creates a dictionary with an entry for each view in the container mapping.</span></span> <span data-ttu-id="f98ed-129">Die zweite Methode berechnet einen Hashwert für die einzelne Container Zuordnung und wird zur Laufzeit verwendet, um zu überprüfen, ob das Modell nicht geändert wurde, seit die Ansichten vorab generiert wurden.</span><span class="sxs-lookup"><span data-stu-id="f98ed-129">The second method computes a hash value for the single container mapping and is used at runtime to validate that the model has not changed since the views were pre-generated.</span></span> <span data-ttu-id="f98ed-130">Über schreibungen der beiden Methoden werden für komplexe Szenarien mit mehreren Container Zuordnungen bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="f98ed-130">Overrides of the two methods are provided for complex scenarios involving multiple container mappings.</span></span>

<span data-ttu-id="f98ed-131">Beim Generieren von Sichten rufen Sie die GenerateViews-Methode auf und schreiben dann die resultierenden EntitySetBase und dbmappingview.</span><span class="sxs-lookup"><span data-stu-id="f98ed-131">When generating views you will call the GenerateViews method and then write out the resulting EntitySetBase and DbMappingView.</span></span> <span data-ttu-id="f98ed-132">Außerdem müssen Sie den von der computemappinghashvalue-Methode generierten Hash speichern.</span><span class="sxs-lookup"><span data-stu-id="f98ed-132">You will also need to store the hash generated by the ComputeMappingHashValue method.</span></span>

### <a name="loading-views"></a><span data-ttu-id="f98ed-133">Sichten werden geladen</span><span class="sxs-lookup"><span data-stu-id="f98ed-133">Loading Views</span></span>

<span data-ttu-id="f98ed-134">Um die von der GenerateViews-Methode generierten Sichten zu laden, können Sie EF eine Klasse bereitstellen, die von der abstrakten Klasse dbmappingviewcache erbt.</span><span class="sxs-lookup"><span data-stu-id="f98ed-134">In order to load the views generated by the GenerateViews method, you can provide EF with a class that inherits from the DbMappingViewCache abstract class.</span></span> <span data-ttu-id="f98ed-135">Dbmappingviewcache gibt zwei Methoden an, die Sie implementieren müssen:</span><span class="sxs-lookup"><span data-stu-id="f98ed-135">DbMappingViewCache specifies two methods that you must implement:</span></span>

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

<span data-ttu-id="f98ed-136">Die mappinghashvalue-Eigenschaft muss den von der computemappinghashvalue-Methode generierten Hash zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="f98ed-136">The MappingHashValue property must return the hash generated by the ComputeMappingHashValue method.</span></span> <span data-ttu-id="f98ed-137">Wenn EF nach Sichten fragt, wird zunächst der Hashwert des Modells mit dem von dieser Eigenschaft zurückgegebenen Hash generiert und verglichen.</span><span class="sxs-lookup"><span data-stu-id="f98ed-137">When EF is going to ask for views it will first generate and compare the hash value of the model with the hash returned by this property.</span></span> <span data-ttu-id="f98ed-138">Wenn keine Entsprechung gefunden wird, löst EF eine EntityCommandCompilationException-Ausnahme aus.</span><span class="sxs-lookup"><span data-stu-id="f98ed-138">If they do not match then EF will throw an EntityCommandCompilationException exception.</span></span>

<span data-ttu-id="f98ed-139">Die GetView-Methode akzeptiert ein EntitySetBase-Objekt, und Sie müssen eine dbmappingview mit der EntitySql-ID zurückgeben, die für generiert wurde, die dem angegebenen EntitySetBase in dem von der GenerateViews-Methode generierten Wörterbuch zugeordnet ist.</span><span class="sxs-lookup"><span data-stu-id="f98ed-139">The GetView method will accept an EntitySetBase and you need to return a DbMappingVIew containing the EntitySql that was generated for that was associated with the given EntitySetBase in the dictionary generated by the GenerateViews method.</span></span> <span data-ttu-id="f98ed-140">Wenn EF nach einer Ansicht fragt, die Sie nicht haben, sollte GetView NULL zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="f98ed-140">If EF asks for a view that you do not have then GetView should return null.</span></span>

<span data-ttu-id="f98ed-141">Im folgenden finden Sie eine Extraktion aus dbmappingviewcache, die mit den oben beschriebenen Power Tools generiert wird. darin wird eine Möglichkeit zum Speichern und Abrufen des erforderlichen EntitySql-Werts angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f98ed-141">The following is an extract from the DbMappingViewCache that is generated with the Power Tools as described above, in it we see one way to store and retrieve the EntitySql required.</span></span>

``` csharp
    public override string MappingHashValue
    {
        get { return "a0b843f03dd29abee99789e190a6fb70ce8e93dc97945d437d9a58fb8e2afd2e"; }
    }

    public override DbMappingView GetView(EntitySetBase extent)
    {
        if (extent == null)
        {
            throw new ArgumentNullException("extent");
        }

        var extentName = extent.EntityContainer.Name + "." + extent.Name;

        if (extentName == "BlogContext.Blogs")
        {
            return GetView2();
        }

        if (extentName == "BlogContext.Posts")
        {
            return GetView3();
        }

        return null;
    }

    private static DbMappingView GetView2()
    {
        return new DbMappingView(@"
            SELECT VALUE -- Constructing Blogs
            [BlogApp.Models.Blog](T1.Blog_BlogId, T1.Blog_Test, T1.Blog_title, T1.Blog_Active, T1.Blog_SomeDecimal)
            FROM (
            SELECT
                T.BlogId AS Blog_BlogId,
                T.Test AS Blog_Test,
                T.title AS Blog_title,
                T.Active AS Blog_Active,
                T.SomeDecimal AS Blog_SomeDecimal,
                True AS _from0
            FROM CodeFirstDatabase.Blog AS T
            ) AS T1");
    }
```

<span data-ttu-id="f98ed-142">Damit EF ihren dbmappingviewcache verwenden kann, fügen Sie das dbmappingviewcachetypeattribute-Attribut hinzu, indem Sie den Kontext angeben, für den es erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="f98ed-142">To have EF use your DbMappingViewCache you add use the DbMappingViewCacheTypeAttribute, specifying the context that it was created for.</span></span> <span data-ttu-id="f98ed-143">Im folgenden Code ordnen wir den blogcontext der mymappingviewcache-Klasse zu.</span><span class="sxs-lookup"><span data-stu-id="f98ed-143">In the code below we associate the BlogContext with the MyMappingViewCache class.</span></span>

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

<span data-ttu-id="f98ed-144">Für komplexere Szenarien können Zuordnungs Ansichts Cache Instanzen bereitgestellt werden, indem eine Zuordnungs Ansichts Cache-Factory angegeben wird.</span><span class="sxs-lookup"><span data-stu-id="f98ed-144">For more complex scenarios, mapping view cache instances can be provided by specifying a mapping view cache factory.</span></span> <span data-ttu-id="f98ed-145">Dies kann durch Implementieren der abstrakten Klasse System. Data. Entity. Infrastructure. mappingviews. dbmappingviewcachefactory erreicht werden.</span><span class="sxs-lookup"><span data-stu-id="f98ed-145">This can be done by implementing the abstract class System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span></span> <span data-ttu-id="f98ed-146">Die Instanz der Cache Factory der Zuordnungs Ansicht, die verwendet wird, kann mithilfe von StorageMappingItemCollection. mappingviewcachefactoryproperty abgerufen oder festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="f98ed-146">The instance of the mapping view cache factory that is used can be retrieved or set using the StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span></span>
