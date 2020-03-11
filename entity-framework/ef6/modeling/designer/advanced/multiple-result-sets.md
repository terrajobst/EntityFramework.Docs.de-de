---
title: Gespeicherte Prozeduren mit mehreren Resultsets EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
ms.openlocfilehash: 098ed88ba52e211965baf3660f0e51bd74c71efd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415454"
---
# <a name="stored-procedures-with-multiple-result-sets"></a><span data-ttu-id="5ffba-102">Gespeicherte Prozeduren mit mehreren Resultsets</span><span class="sxs-lookup"><span data-stu-id="5ffba-102">Stored Procedures with Multiple Result Sets</span></span>
<span data-ttu-id="5ffba-103">Manchmal müssen Sie, wenn Sie gespeicherte Prozeduren verwenden, mehr als ein Resultset zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="5ffba-103">Sometimes when using stored procedures you will need to return more than one result set.</span></span> <span data-ttu-id="5ffba-104">Dieses Szenario wird häufig verwendet, um die Anzahl der Daten Bank Roundtrips zu reduzieren, die zum Verfassen eines einzelnen Bildschirms erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="5ffba-104">This scenario is commonly used to reduce the number of database round trips required to compose a single screen.</span></span><span data-ttu-id="5ffba-105"> Vor EF5 konnte Entity Framework die gespeicherte Prozedur aufrufen, aber nur das erste Resultset an den aufrufenden Code zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="5ffba-105"> Prior to EF5, Entity Framework would allow the stored procedure to be called but would only return the first result set to the calling code.</span></span>

<span data-ttu-id="5ffba-106">In diesem Artikel werden zwei Möglichkeiten für den Zugriff auf mehr als ein Resultset aus einer gespeicherten Prozedur in Entity Framework erläutert.</span><span class="sxs-lookup"><span data-stu-id="5ffba-106">This article will show you two ways that you can use to access more than one result set from a stored procedure in Entity Framework.</span></span> <span data-ttu-id="5ffba-107">Eine, die nur Code verwendet und sowohl mit Code First als auch mit dem EF-Designer und einem, der nur mit dem EF-Designer funktioniert, funktioniert.</span><span class="sxs-lookup"><span data-stu-id="5ffba-107">One that uses just code and works with both Code first and the EF Designer and one that only works with the EF Designer.</span></span> <span data-ttu-id="5ffba-108">Die Tools und API-Unterstützung für diese sollte in zukünftigen Versionen von Entity Framework verbessert werden.</span><span class="sxs-lookup"><span data-stu-id="5ffba-108">The tooling and API support for this should improve in future versions of Entity Framework.</span></span>

## <a name="model"></a><span data-ttu-id="5ffba-109">Modell</span><span class="sxs-lookup"><span data-stu-id="5ffba-109">Model</span></span>

<span data-ttu-id="5ffba-110">In den Beispielen in diesem Artikel wird ein grundlegender Blog verwendet, bei dem ein Blog viele Beiträge enthält und ein Beitrag zu einem einzelnen Blog gehört.</span><span class="sxs-lookup"><span data-stu-id="5ffba-110">The examples in this article use a basic Blog and Posts model where a blog has many posts and a post belongs to a single blog.</span></span> <span data-ttu-id="5ffba-111">Wir verwenden eine gespeicherte Prozedur in der Datenbank, die alle Blogs und Beiträge wie folgt zurückgibt:</span><span class="sxs-lookup"><span data-stu-id="5ffba-111">We will use a stored procedure in the database that returns all blogs and posts, something like this:</span></span>

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a><span data-ttu-id="5ffba-112">Zugreifen auf mehrere Resultsets mit Code</span><span class="sxs-lookup"><span data-stu-id="5ffba-112">Accessing Multiple Result Sets with Code</span></span>

<span data-ttu-id="5ffba-113">Mithilfe von Code können Sie einen unformatierten SQL-Befehl ausführen, um die gespeicherte Prozedur auszuführen.</span><span class="sxs-lookup"><span data-stu-id="5ffba-113">We can execute use code to issue a raw SQL command to execute our stored procedure.</span></span> <span data-ttu-id="5ffba-114">Der Vorteil dieses Ansatzes besteht darin, dass er sowohl mit Code First als auch mit dem EF-Designer funktioniert.</span><span class="sxs-lookup"><span data-stu-id="5ffba-114">The advantage of this approach is that it works with both Code first and the EF Designer.</span></span>

<span data-ttu-id="5ffba-115">Damit mehrere Resultsets funktionieren, müssen wir die ObjectContext-API mithilfe der iobjectcontextadapter-Schnittstelle löschen.</span><span class="sxs-lookup"><span data-stu-id="5ffba-115">In order to get multiple result sets working we need to drop to the ObjectContext API by using the IObjectContextAdapter interface.</span></span>

<span data-ttu-id="5ffba-116">Nachdem wir einen ObjectContext haben, können wir die Übersetzung-Methode verwenden, um die Ergebnisse der gespeicherten Prozedur in Entitäten zu übersetzen, die in EF als normal verfolgt und verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="5ffba-116">Once we have an ObjectContext then we can use the Translate method to translate the results of our stored procedure into entities that can be tracked and used in EF as normal.</span></span> <span data-ttu-id="5ffba-117">Im folgenden Codebeispiel wird dies in Aktion veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="5ffba-117">The following code sample demonstrates this in action.</span></span>

``` csharp
    using (var db = new BloggingContext())
    {
        // If using Code First we need to make sure the model is built before we open the connection
        // This isn't required for models created with the EF Designer
        db.Database.Initialize(force: false);

        // Create a SQL command to execute the sproc
        var cmd = db.Database.Connection.CreateCommand();
        cmd.CommandText = "[dbo].[GetAllBlogsAndPosts]";

        try
        {

            db.Database.Connection.Open();
            // Run the sproc
            var reader = cmd.ExecuteReader();

            // Read Blogs from the first result set
            var blogs = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Blog>(reader, "Blogs", MergeOption.AppendOnly);   


            foreach (var item in blogs)
            {
                Console.WriteLine(item.Name);
            }        

            // Move to second result set and read Posts
            reader.NextResult();
            var posts = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Post>(reader, "Posts", MergeOption.AppendOnly);


            foreach (var item in posts)
            {
                Console.WriteLine(item.Title);
            }
        }
        finally
        {
            db.Database.Connection.Close();
        }
    }
```

<span data-ttu-id="5ffba-118">Die Translation-Methode akzeptiert den Reader, den wir beim Ausführen der Prozedur erhalten haben, einen EntitySet-Namen und eine MergeOption.</span><span class="sxs-lookup"><span data-stu-id="5ffba-118">The Translate method accepts the reader that we received when we executed the procedure, an EntitySet name, and a MergeOption.</span></span> <span data-ttu-id="5ffba-119">Der EntitySet-Name ist identisch mit der dbset-Eigenschaft für den abgeleiteten Kontext.</span><span class="sxs-lookup"><span data-stu-id="5ffba-119">The EntitySet name will be the same as the DbSet property on your derived context.</span></span> <span data-ttu-id="5ffba-120">Die MergeOption-Aufzählung steuert, wie Ergebnisse behandelt werden, wenn die gleiche Entität bereits im Arbeitsspeicher vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="5ffba-120">The MergeOption enum controls how results are handled if the same entity already exists in memory.</span></span>

<span data-ttu-id="5ffba-121">Hier durchlaufen wir die Auflistung von Blogs, bevor wir NextResult nennen. Dies ist für den obigen Code wichtig, da das erste Resultset vor dem Wechsel zum nächsten Resultset verarbeitet werden muss.</span><span class="sxs-lookup"><span data-stu-id="5ffba-121">Here we iterate through the collection of blogs before we call NextResult, this is important given the above code because the first result set must be consumed before moving to the next result set.</span></span>

<span data-ttu-id="5ffba-122">Nachdem die beiden Übersetzungsmethoden aufgerufen wurden, werden die Blog-und Post-Entitäten von EF auf die gleiche Weise wie jede andere Entität nachverfolgt und können daher geändert oder gelöscht und als normal gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="5ffba-122">Once the two translate methods are called then the Blog and Post entities are tracked by EF the same way as any other entity and so can be modified or deleted and saved as normal.</span></span>

>[!NOTE]
> <span data-ttu-id="5ffba-123">EF nimmt beim Erstellen von Entitäten mithilfe der Translation-Methode keine Zuordnung zu einem Konto vor.</span><span class="sxs-lookup"><span data-stu-id="5ffba-123">EF does not take any mapping into account when it creates entities using the Translate method.</span></span> <span data-ttu-id="5ffba-124">Es werden einfach die Spaltennamen im Resultset mit Eigenschaftsnamen für Ihre Klassen abgeglichen.</span><span class="sxs-lookup"><span data-stu-id="5ffba-124">It will simply match column names in the result set with property names on your classes.</span></span>

>[!NOTE]
> <span data-ttu-id="5ffba-125">Wenn Sie Lazy Loading aktiviert haben und auf die Posts-Eigenschaft für eine der Blog Entitäten zugreifen, stellt EF eine Verbindung mit der Datenbank her, um alle Beiträge verzögert zu laden, obwohl wir Sie bereits alle geladen haben.</span><span class="sxs-lookup"><span data-stu-id="5ffba-125">That if you have lazy loading enabled, accessing the posts property on one of the blog entities then EF will connect to the database to lazily load all posts, even though we have already loaded them all.</span></span> <span data-ttu-id="5ffba-126">Dies liegt daran, dass EF nicht weiß, ob Sie alle Beiträge geladen haben oder ob in der Datenbank weitere vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="5ffba-126">This is because EF cannot know whether or not you have loaded all posts or if there are more in the database.</span></span> <span data-ttu-id="5ffba-127">Wenn Sie dies vermeiden möchten, müssen Sie Lazy Loading deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="5ffba-127">If you want to avoid this then you will need to disable lazy loading.</span></span>

## <a name="multiple-result-sets-with-configured-in-edmx"></a><span data-ttu-id="5ffba-128">Mehrere Resultsets mit Konfiguration in EDMX</span><span class="sxs-lookup"><span data-stu-id="5ffba-128">Multiple Result Sets with Configured in EDMX</span></span>

>[!NOTE]
> <span data-ttu-id="5ffba-129">Sie müssen .NET Framework 4,5 als Ziel festlegen, um mehrere Resultsets in edmx konfigurieren zu können.</span><span class="sxs-lookup"><span data-stu-id="5ffba-129">You must target .NET Framework 4.5 to be able to configure multiple result sets in EDMX.</span></span> <span data-ttu-id="5ffba-130">Wenn Sie .NET 4,0 als Ziel verwenden, können Sie die im vorherigen Abschnitt gezeigte Code basierte Methode verwenden.</span><span class="sxs-lookup"><span data-stu-id="5ffba-130">If you are targeting .NET 4.0 you can use the code-based method shown in the previous section.</span></span>

<span data-ttu-id="5ffba-131">Wenn Sie den EF-Designer verwenden, können Sie das Modell auch so ändern, dass es die unterschiedlichen Resultsets kennt, die zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="5ffba-131">If you are using the EF Designer, you can also modify your model so that it knows about the different result sets that will be returned.</span></span> <span data-ttu-id="5ffba-132">Ein wichtiger Aspekt ist, dass die Tools nicht mehrere Resultsets unterstützen, sodass Sie die EDMX-Datei manuell bearbeiten müssen.</span><span class="sxs-lookup"><span data-stu-id="5ffba-132">One thing to know before hand is that the tooling is not multiple result set aware, so you will need to manually edit the edmx file.</span></span> <span data-ttu-id="5ffba-133">Wenn Sie die EDMX-Datei wie folgt bearbeiten, wird auch die Überprüfung des Modells in vs nicht unterbricht.</span><span class="sxs-lookup"><span data-stu-id="5ffba-133">Editing the edmx file like this will work but it will also break the validation of the model in VS.</span></span> <span data-ttu-id="5ffba-134">Wenn Sie also das Modell validieren, erhalten Sie immer Fehler.</span><span class="sxs-lookup"><span data-stu-id="5ffba-134">So if you validate your model you will always get errors.</span></span>

-   <span data-ttu-id="5ffba-135">Um dies zu erreichen, müssen Sie die gespeicherte Prozedur wie bei einer einzelnen resultsetabfrage zum Modell hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="5ffba-135">In order to do this you need to add the stored procedure to your model as you would for a single result set query.</span></span>
-   <span data-ttu-id="5ffba-136">Wenn Sie diese haben, klicken Sie mit der rechten Maustaste auf das Modell, und wählen Sie **Öffnen mit aus.**</span><span class="sxs-lookup"><span data-stu-id="5ffba-136">Once you have this then you need to right click on your model and select **Open With..**</span></span> <span data-ttu-id="5ffba-137">**XML** -Datei</span><span class="sxs-lookup"><span data-stu-id="5ffba-137">then **Xml**</span></span>

    ![Öffnen als](~/ef6/media/openas.png)

<span data-ttu-id="5ffba-139">Nachdem Sie das Modell als XML geöffnet haben, müssen Sie die folgenden Schritte ausführen:</span><span class="sxs-lookup"><span data-stu-id="5ffba-139">Once you have the model opened as XML then you need to do the following steps:</span></span>

-   <span data-ttu-id="5ffba-140">Suchen Sie den komplexen Typ und den Funktions Import in Ihrem Modell:</span><span class="sxs-lookup"><span data-stu-id="5ffba-140">Find the complex type and function import in your model:</span></span>

``` xml
    <!-- CSDL content -->
    <edmx:ConceptualModels>

    ...

      <FunctionImport Name="GetAllBlogsAndPosts" ReturnType="Collection(BlogModel.GetAllBlogsAndPosts_Result)" />

    ...

      <ComplexType Name="GetAllBlogsAndPosts_Result">
        <Property Type="Int32" Name="BlogId" Nullable="false" />
        <Property Type="String" Name="Name" Nullable="false" MaxLength="255" />
        <Property Type="String" Name="Description" Nullable="true" />
      </ComplexType>

    ...

    </edmx:ConceptualModels>
```

 

-   <span data-ttu-id="5ffba-141">Entfernen des komplexen Typs</span><span class="sxs-lookup"><span data-stu-id="5ffba-141">Remove the complex type</span></span>
-   <span data-ttu-id="5ffba-142">Aktualisieren Sie den Funktions Import so, dass er ihren Entitäten zugeordnet wird. in diesem Fall sieht er wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="5ffba-142">Update the function import so that it maps to your entities, in our case it will look like the following:</span></span>

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

<span data-ttu-id="5ffba-143">Dadurch wird dem Modell mitgeteilt, dass die gespeicherte Prozedur zwei Auflistungen zurückgibt: einen der Blogeinträge und einen der Post-Einträge.</span><span class="sxs-lookup"><span data-stu-id="5ffba-143">This tells the model that the stored procedure will return two collections, one of blog entries and one of post entries.</span></span>

-   <span data-ttu-id="5ffba-144">Suchen Sie das Funktions Zuordnungs Element:</span><span class="sxs-lookup"><span data-stu-id="5ffba-144">Find the function mapping element:</span></span>

``` xml
    <!-- C-S mapping content -->
    <edmx:Mappings>

    ...

      <FunctionImportMapping FunctionImportName="GetAllBlogsAndPosts" FunctionName="BlogModel.Store.GetAllBlogsAndPosts">
        <ResultMapping>
          <ComplexTypeMapping TypeName="BlogModel.GetAllBlogsAndPosts_Result">
            <ScalarProperty Name="BlogId" ColumnName="BlogId" />
            <ScalarProperty Name="Name" ColumnName="Name" />
            <ScalarProperty Name="Description" ColumnName="Description" />
          </ComplexTypeMapping>
        </ResultMapping>
      </FunctionImportMapping>

    ...

    </edmx:Mappings>
```

-   <span data-ttu-id="5ffba-145">Ersetzen Sie die Ergebnis Zuordnung durch eine für jede Entität, die zurückgegeben wird, wie z. b. Folgendes:</span><span class="sxs-lookup"><span data-stu-id="5ffba-145">Replace the result mapping with one for each entity being returned, such as the following:</span></span>

``` xml
    <ResultMapping>
      <EntityTypeMapping TypeName ="BlogModel.Blog">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="Name" ColumnName="Name" />
        <ScalarProperty Name="Description" ColumnName="Description" />
      </EntityTypeMapping>
    </ResultMapping>
    <ResultMapping>
      <EntityTypeMapping TypeName="BlogModel.Post">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="PostId" ColumnName="PostId"/>
        <ScalarProperty Name="Title" ColumnName="Title" />
        <ScalarProperty Name="Text" ColumnName="Text" />
      </EntityTypeMapping>
    </ResultMapping>
```

<span data-ttu-id="5ffba-146">Es ist auch möglich, die Resultsets komplexen Typen zuzuordnen, z. b. der standardmäßig erstellten.</span><span class="sxs-lookup"><span data-stu-id="5ffba-146">It is also possible to map the result sets to complex types, such as the one created by default.</span></span> <span data-ttu-id="5ffba-147">Zu diesem Zweck erstellen Sie einen neuen komplexen Typ, anstatt ihn zu entfernen, und verwenden die komplexen Typen überall dort, wo Sie die Entitäts Namen in den obigen Beispielen verwendet haben.</span><span class="sxs-lookup"><span data-stu-id="5ffba-147">To do this you create a new complex type, instead of removing them, and use the complex types everywhere that you had used the entity names in the examples above.</span></span>

<span data-ttu-id="5ffba-148">Nachdem diese Zuordnungen geändert wurden, können Sie das Modell speichern und den folgenden Code ausführen, um die gespeicherte Prozedur zu verwenden:</span><span class="sxs-lookup"><span data-stu-id="5ffba-148">Once these mappings have been changed then you can save the model and execute the following code to use the stored procedure:</span></span>

``` csharp
    using (var db = new BlogEntities())
    {
        var results = db.GetAllBlogsAndPosts();

        foreach (var result in results)
        {
            Console.WriteLine("Blog: " + result.Name);
        }

        var posts = results.GetNextResult<Post>();

        foreach (var result in posts)
        {
            Console.WriteLine("Post: " + result.Title);
        }

        Console.ReadLine();
    }
```

>[!NOTE]
> <span data-ttu-id="5ffba-149">Wenn Sie die EDMX-Datei für das Modell manuell bearbeiten, wird sie überschrieben, wenn Sie das Modell jemals aus der Datenbank generieren.</span><span class="sxs-lookup"><span data-stu-id="5ffba-149">If you manually edit the edmx file for your model it will be overwritten if you ever regenerate the model from the database.</span></span>

## <a name="summary"></a><span data-ttu-id="5ffba-150">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="5ffba-150">Summary</span></span>

<span data-ttu-id="5ffba-151">Hier haben wir zwei verschiedene Methoden für den Zugriff auf mehrere Resultsets mit Entity Framework gezeigt.</span><span class="sxs-lookup"><span data-stu-id="5ffba-151">Here we have shown two different methods of accessing multiple result sets using Entity Framework.</span></span> <span data-ttu-id="5ffba-152">Beide sind abhängig von ihrer Situation und ihren Vorlieben gleichermaßen gültig, und Sie sollten den Wert auswählen, der für ihre Umstände am besten geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="5ffba-152">Both of them are equally valid depending on your situation and preferences and you should choose the one that seems best for your circumstances.</span></span> <span data-ttu-id="5ffba-153">Es ist geplant, dass die Unterstützung für mehrere Resultsets in zukünftigen Versionen von Entity Framework verbessert wird und dass das Ausführen der Schritte in diesem Dokument nicht mehr erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="5ffba-153">It is planned that the support for multiple result sets will be improved in future versions of Entity Framework and that performing the steps in this document will no longer be necessary.</span></span>
