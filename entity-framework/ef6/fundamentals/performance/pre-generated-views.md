---
title: Vorab generierte Mapping-Ansichten – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
ms.openlocfilehash: 1fda9fe9638adce9b24a6b81aa081effeb0def81
ms.sourcegitcommit: c568d33214fc25c76e02c8529a29da7a356b37b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2018
ms.locfileid: "47459525"
---
# <a name="pre-generated-mapping-views"></a><span data-ttu-id="012f9-102">Vorab generierte Mapping-Ansichten</span><span class="sxs-lookup"><span data-stu-id="012f9-102">Pre-generated mapping views</span></span>
<span data-ttu-id="012f9-103">Bevor das Entity Framework einer Abfrage ausführen oder Änderungen an der Datenquelle speichern können, müssen sie einen Satz von Ansichten der Zuordnung Zugriff auf die Datenbank erstellen.</span><span class="sxs-lookup"><span data-stu-id="012f9-103">Before the Entity Framework can execute a query or save changes to the data source, it must generate a set of mapping views to access the database.</span></span> <span data-ttu-id="012f9-104">Diese Zuordnung Ansichten sind eine Reihe von Entity SQL-Anweisung, die die Datenbank auf abstrakte Weise darstellen, und sind Teil der Metadaten, die pro Anwendungsdomäne zwischengespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="012f9-104">These mapping views are a set of Entity SQL statement that represent the database in an abstract way, and are part of the metadata which is cached per application domain.</span></span> <span data-ttu-id="012f9-105">Wenn Sie mehrere Instanzen desselben Kontexts in der gleichen Anwendungsdomäne erstellen, werden diese Zuordnung Ansichten aus objektkontextinstanzen, statt die zwischengespeicherten Metadaten wiederverwendet.</span><span class="sxs-lookup"><span data-stu-id="012f9-105">If you create multiple instances of the same context in the same application domain, they will reuse mapping views from the cached metadata rather than regenerating them.</span></span> <span data-ttu-id="012f9-106">Da Zuordnung Generieren von Sichten einen signifikanten Teil der Gesamtkosten der Ausführung der ersten Abfrage ist, ermöglicht dem Entity Framework vorgenerieren von Ansichten der Zuordnung, und schließen Sie sie in das kompilierte Projekt an.</span><span class="sxs-lookup"><span data-stu-id="012f9-106">Because mapping view generation is a significant part of the overall cost of executing the first query, the Entity Framework enables you to pre-generate mapping views and include them in the compiled project.</span></span> <span data-ttu-id="012f9-107">Weitere Informationen finden Sie unter [Überlegungen zur Leistung (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span><span class="sxs-lookup"><span data-stu-id="012f9-107">For more information, see  [Performance Considerations (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span></span>

## <a name="generating-mapping-views-with-the-ef-power-tools-community-edition"></a><span data-ttu-id="012f9-108">Generieren von Zuordnen von Ansichten mit der EF Power Tools-Community-Edition</span><span class="sxs-lookup"><span data-stu-id="012f9-108">Generating Mapping Views with the EF Power Tools Community Edition</span></span>

<span data-ttu-id="012f9-109">Die einfachste Möglichkeit zum vorgenerieren von Ansichten ist die Verwendung der [EF Power Tools-Community-Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition).</span><span class="sxs-lookup"><span data-stu-id="012f9-109">The easiest way to pre-generate views is to use the [EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition).</span></span> <span data-ttu-id="012f9-110">Nachdem Sie die Power-Tools installiert haben, müssen Sie eine Menüoption zum Generieren von Sichten, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="012f9-110">Once you have the Power Tools installed you will have a menu option to Generate Views, as below.</span></span>

-   <span data-ttu-id="012f9-111">Für **Code First** Modelle mit der rechten Maustaste auf die Codedatei, die Ihr "DbContext"-Klasse enthält.</span><span class="sxs-lookup"><span data-stu-id="012f9-111">For **Code First** models right-click on the code file that contains your DbContext class.</span></span>
-   <span data-ttu-id="012f9-112">Für **EF Designer** Modelle mit der rechten Maustaste auf die EDMX-Datei.</span><span class="sxs-lookup"><span data-stu-id="012f9-112">For **EF Designer** models right-click on your EDMX file.</span></span>

![Generieren von Sichten](~/ef6/media/generateviews.png)

<span data-ttu-id="012f9-114">Sobald der Vorgang abgeschlossen ist, müssen Sie eine Klasse, die etwa wie folgt generiert</span><span class="sxs-lookup"><span data-stu-id="012f9-114">Once the process is finished you will have a class similar to the following generated</span></span>

![generierte Ansichten](~/ef6/media/generatedviews.png)

<span data-ttu-id="012f9-116">Jetzt beim Ausführen verwendet Ihrer Anwendung EF diese Klasse Ansichten nach Bedarf geladen.</span><span class="sxs-lookup"><span data-stu-id="012f9-116">Now when you run your application EF will use this class to load views as required.</span></span> <span data-ttu-id="012f9-117">Wenn Ihr Modell ändert und Sie nicht diese Klasse erneut generieren, wird EF eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="012f9-117">If your model changes and you do not re-generate this class then EF will throw an exception.</span></span>

## <a name="generating-mapping-views-from-code---ef6-onwards"></a><span data-ttu-id="012f9-118">Generieren von Zuordnen von Ansichten von Code – EF6 oder höher</span><span class="sxs-lookup"><span data-stu-id="012f9-118">Generating Mapping Views from Code - EF6 Onwards</span></span>

<span data-ttu-id="012f9-119">Die andere Möglichkeit zum Generieren von Sichten ist zur Verwendung der APIs, die EF bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="012f9-119">The other way to generate views is to use the APIs that EF provides.</span></span> <span data-ttu-id="012f9-120">Bei Verwendung dieser Methode müssen Sie die Freiheit gibt, die Ansichten zu serialisieren, aber wie Sie allerdings Sie auch die Ansichten selbst laden müssen.</span><span class="sxs-lookup"><span data-stu-id="012f9-120">When using this method you have the freedom to serialize the views however you like, but you also need to load the views yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="012f9-121">**EF6 oder höher, nur** -der APIs, die in diesem Abschnitt gezeigten in Entity Framework 6 eingeführt wurden.</span><span class="sxs-lookup"><span data-stu-id="012f9-121">**EF6 Onwards Only** - The APIs shown in this section were introduced in Entity Framework 6.</span></span> <span data-ttu-id="012f9-122">Diese Informationen gelten nicht, wenn Sie eine frühere Version verwenden.</span><span class="sxs-lookup"><span data-stu-id="012f9-122">If you are using an earlier version this information does not apply.</span></span>

### <a name="generating-views"></a><span data-ttu-id="012f9-123">Generieren von Sichten</span><span class="sxs-lookup"><span data-stu-id="012f9-123">Generating Views</span></span>

<span data-ttu-id="012f9-124">Die APIs zum Generieren von Ansichten sind für die System.Data.Entity.Core.Mapping.StorageMappingItemCollection-Klasse.</span><span class="sxs-lookup"><span data-stu-id="012f9-124">The APIs to generate views are on the System.Data.Entity.Core.Mapping.StorageMappingItemCollection class.</span></span> <span data-ttu-id="012f9-125">Sie können eine StorageMappingCollection für einen Kontext abrufen, mit der MetadataWorkspace von ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="012f9-125">You can retrieve a StorageMappingCollection for a Context by using the MetadataWorkspace of an ObjectContext.</span></span> <span data-ttu-id="012f9-126">Wenn Sie die neueren DbContext-API verwenden, können Sie dies mithilfe von auf zugreifen können der IObjectContextAdapter wie die folgende, in diesem Code haben wir eine Instanz von Ihr abgeleiteten "DbContext" und "DbContext" aufgerufen:</span><span class="sxs-lookup"><span data-stu-id="012f9-126">If you are using the newer DbContext API then you can access this by using the IObjectContextAdapter like below, in this code we have an instance of your derived DbContext called dbContext:</span></span>

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

<span data-ttu-id="012f9-127">Nachdem Sie die StorageMappingItemCollection haben, erhalten Sie Zugriff auf die GenerateViews und ComputeMappingHashValue-Methoden.</span><span class="sxs-lookup"><span data-stu-id="012f9-127">Once you have the StorageMappingItemCollection then you can get access to the GenerateViews and ComputeMappingHashValue methods.</span></span>

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

<span data-ttu-id="012f9-128">Die erste Methode erstellt ein Wörterbuch mit einem Eintrag für jede Ansicht in der Zuordnung des Containers.</span><span class="sxs-lookup"><span data-stu-id="012f9-128">The first method creates a dictionary with an entry for each view in the container mapping.</span></span> <span data-ttu-id="012f9-129">Die zweite Methode berechnet einen Hashwert für die Zuordnung eines einzelnen Containers und dient zur Laufzeit zum Überprüfen, dass das Modell nicht geändert wurde, da die Ansichten vorab generiert wurden.</span><span class="sxs-lookup"><span data-stu-id="012f9-129">The second method computes a hash value for the single container mapping and is used at runtime to validate that the model has not changed since the views were pre-generated.</span></span> <span data-ttu-id="012f9-130">Überschreibungen der beiden Methoden werden für komplexe Szenarien im Zusammenhang mit mehreren schutzcontainerzuordnungen bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="012f9-130">Overrides of the two methods are provided for complex scenarios involving multiple container mappings.</span></span>

<span data-ttu-id="012f9-131">Beim Generieren von Sichten, die Sie GenerateViews Methode aufrufen, und klicken Sie dann die resultierende EntitySetBase und DbMappingView schreiben werden.</span><span class="sxs-lookup"><span data-stu-id="012f9-131">When generating views you will call the GenerateViews method and then write out the resulting EntitySetBase and DbMappingView.</span></span> <span data-ttu-id="012f9-132">Sie müssen auch die von der-Methode ComputeMappingHashValue generierten Hash speichern.</span><span class="sxs-lookup"><span data-stu-id="012f9-132">You will also need to store the hash generated by the ComputeMappingHashValue method.</span></span>

### <a name="loading-views"></a><span data-ttu-id="012f9-133">Laden von Ansichten</span><span class="sxs-lookup"><span data-stu-id="012f9-133">Loading Views</span></span>

<span data-ttu-id="012f9-134">Um die von der-Methode GenerateViews generierten Ansichten zu laden, können Sie EF mit einer Klasse bereitstellen, die von der abstrakten DbMappingViewCache-Klasse erbt.</span><span class="sxs-lookup"><span data-stu-id="012f9-134">In order to load the views generated by the GenerateViews method, you can provide EF with a class that inherits from the DbMappingViewCache abstract class.</span></span> <span data-ttu-id="012f9-135">DbMappingViewCache gibt zwei Methoden, die Sie implementieren müssen:</span><span class="sxs-lookup"><span data-stu-id="012f9-135">DbMappingViewCache specifies two methods that you must implement:</span></span>

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

<span data-ttu-id="012f9-136">Die MappingHashValue-Eigenschaft muss den von der-Methode ComputeMappingHashValue generierten Hash zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="012f9-136">The MappingHashValue property must return the hash generated by the ComputeMappingHashValue method.</span></span> <span data-ttu-id="012f9-137">Wenn EF ist jetzt für Ansichten stellen sie zunächst generiert und vergleichen den Hashwert des Modells mit dem Hashwert, der von dieser Eigenschaft zurückgegebene.</span><span class="sxs-lookup"><span data-stu-id="012f9-137">When EF is going to ask for views it will first generate and compare the hash value of the model with the hash returned by this property.</span></span> <span data-ttu-id="012f9-138">Wenn sie nicht übereinstimmen dann löst EF EntityCommandCompilationException eine Ausnahme.</span><span class="sxs-lookup"><span data-stu-id="012f9-138">If they do not match then EF will throw an EntityCommandCompilationException exception.</span></span>

<span data-ttu-id="012f9-139">Die GetView-Methode akzeptiert eine EntitySetBase aus, und Sie müssen zurückgegeben, dass eine DbMappingVIew, enthält die EntitySql, die dafür generiert wurde der angegebene EntitySetBase in das Wörterbuch, das von der-Methode GenerateViews generiert zugeordnet war.</span><span class="sxs-lookup"><span data-stu-id="012f9-139">The GetView method will accept an EntitySetBase and you need to return a DbMappingVIew containing the EntitySql that was generated for that was associated with the given EntitySetBase in the dictionary generated by the GenerateViews method.</span></span> <span data-ttu-id="012f9-140">Eine Ansicht, dass Sie nicht dann GetView verfügen sollte null zurückgeben, wenn EF anfordert.</span><span class="sxs-lookup"><span data-stu-id="012f9-140">If EF asks for a view that you do not have then GetView should return null.</span></span>

<span data-ttu-id="012f9-141">Im folgenden finden einen Auszug aus der DbMappingViewCache, die wie oben beschrieben sehen Sie eine Möglichkeit zum Speichern und Abrufen der EntitySql erforderlich sind, sich mit den Power Tools generiert wird.</span><span class="sxs-lookup"><span data-stu-id="012f9-141">The following is an extract from the DbMappingViewCache that is generated with the Power Tools as described above, in it we see one way to store and retrieve the EntitySql required.</span></span>

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

<span data-ttu-id="012f9-142">Damit EF verwenden Ihre DbMappingViewCache, die Sie hinzufügen, verwenden Sie die DbMappingViewCacheTypeAttribute, Angeben des Kontexts, dem sie für erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="012f9-142">To have EF use your DbMappingViewCache you add use the DbMappingViewCacheTypeAttribute, specifying the context that it was created for.</span></span> <span data-ttu-id="012f9-143">Im folgenden Code ordnen wir den BlogContext, mit der MyMappingViewCache-Klasse.</span><span class="sxs-lookup"><span data-stu-id="012f9-143">In the code below we associate the BlogContext with the MyMappingViewCache class.</span></span>

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

<span data-ttu-id="012f9-144">Für komplexere Szenarien können Zuordnung anzeigen-Cache-Instanzen bereitgestellt werden, durch Angeben einer Zuordnung Ansicht Cache-Factory.</span><span class="sxs-lookup"><span data-stu-id="012f9-144">For more complex scenarios, mapping view cache instances can be provided by specifying a mapping view cache factory.</span></span> <span data-ttu-id="012f9-145">Dies kann durch die Implementierung der abstrakten Klasse System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory erfolgen.</span><span class="sxs-lookup"><span data-stu-id="012f9-145">This can be done by implementing the abstract class System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span></span> <span data-ttu-id="012f9-146">Die Instanz der Zuordnung anzeigen Cache-Factory, die verwendet wird, kann abgerufen oder mithilfe der StorageMappingItemCollection.MappingViewCacheFactoryproperty festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="012f9-146">The instance of the mapping view cache factory that is used can be retrieved or set using the StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span></span>
