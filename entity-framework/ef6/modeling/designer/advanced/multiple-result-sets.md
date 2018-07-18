---
title: Gespeicherte Prozeduren mit mehreren Resultsets – EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
caps.latest.revision: 3
ms.openlocfilehash: 68d544b0c553868ad1ff36cd24db19cff08db073
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121201"
---
# <a name="stored-procedures-with-multiple-result-sets"></a><span data-ttu-id="a726c-102">Gespeicherte Prozeduren mit mehreren Resultsets</span><span class="sxs-lookup"><span data-stu-id="a726c-102">Stored Procedures with Multiple Result Sets</span></span>
<span data-ttu-id="a726c-103">Legen manchmal bei der Verwendung von gespeicherten Prozeduren, die Sie benötigen, um mehr als ein Ergebnis zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="a726c-103">Sometimes when using stored procedures you will need to return more than one result set.</span></span> <span data-ttu-id="a726c-104">Dieses Szenario wird häufig zum Reduzieren der Anzahl der Roundtrips erforderlich, um einen einzelnen Bildschirm zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a726c-104">This scenario is commonly used to reduce the number of database round trips required to compose a single screen.</span></span> <span data-ttu-id="a726c-105">Vor dem EF5 Entity Framework die gespeicherte Prozedur aufgerufen werden können, aber würde nur das erste Resultset an den aufrufenden Code zurück.</span><span class="sxs-lookup"><span data-stu-id="a726c-105">Prior to EF5, Entity Framework would allow the stored procedure to be called but would only return the first result set to the calling code.</span></span>

<span data-ttu-id="a726c-106">In diesem Artikel zeigt Ihnen zwei Möglichkeiten, die Sie verwenden können, auf mehr als ein Resultset aus einer gespeicherten Prozedur im Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a726c-106">This article will show you two ways that you can use to access more than one result set from a stored procedure in Entity Framework.</span></span> <span data-ttu-id="a726c-107">Eine, die nur aus Code verwendet und funktioniert mit beiden Code zuerst und dem EF Designer und eine, die nur mit dem EF Designer funktioniert.</span><span class="sxs-lookup"><span data-stu-id="a726c-107">One that uses just code and works with both Code first and the EF Designer and one that only works with the EF Designer.</span></span> <span data-ttu-id="a726c-108">Die Tools und die API-Unterstützung für diese sollte in zukünftigen Versionen von Entity Framework verbessern.</span><span class="sxs-lookup"><span data-stu-id="a726c-108">The tooling and API support for this should improve in future versions of Entity Framework.</span></span>

## <a name="model"></a><span data-ttu-id="a726c-109">Modell</span><span class="sxs-lookup"><span data-stu-id="a726c-109">Model</span></span>

<span data-ttu-id="a726c-110">In die Beispielen in diesem Artikel verwenden Sie einen einfachen Blog und Beiträge-Modells, die, in denen ein Blog viele Beiträge und eines Beitrags kann, in einem einzelnen Blog gehört.</span><span class="sxs-lookup"><span data-stu-id="a726c-110">The examples in this article use a basic Blog and Posts model where a blog has many posts and a post belongs to a single blog.</span></span> <span data-ttu-id="a726c-111">Wir verwenden eine gespeicherte Prozedur in der Datenbank, die alle Blogs und Beiträgen könnte folgendermaßen aussehen zurückgibt:</span><span class="sxs-lookup"><span data-stu-id="a726c-111">We will use a stored procedure in the database that returns all blogs and posts, something like this:</span></span>

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a><span data-ttu-id="a726c-112">Legt den Zugriff auf mehrere Resultsets, mit Code</span><span class="sxs-lookup"><span data-stu-id="a726c-112">Accessing Multiple Result Sets with Code</span></span>

<span data-ttu-id="a726c-113">Führen wir verwenden von Code zum Ausführen von eine unformatierte SQL‑Befehl aus, um unsere gespeicherte Prozedur auszuführen.</span><span class="sxs-lookup"><span data-stu-id="a726c-113">We can execute use code to issue a raw SQL command to execute our stored procedure.</span></span> <span data-ttu-id="a726c-114">Der Vorteil dieses Ansatzes ist, dass sie zuerst mit sowohl Code funktioniert und dem EF Designer.</span><span class="sxs-lookup"><span data-stu-id="a726c-114">The advantage of this approach is that it works with both Code first and the EF Designer.</span></span>

<span data-ttu-id="a726c-115">Legt fest arbeiten, die wir in der ObjectContext-API durch Ablegen mithilfe der Benutzeroberfläche IObjectContextAdapter müssen, um mehrere Resultsets abzurufen.</span><span class="sxs-lookup"><span data-stu-id="a726c-115">In order to get multiple result sets working we need to drop to the ObjectContext API by using the IObjectContextAdapter interface.</span></span>

<span data-ttu-id="a726c-116">Nachdem wir einen ObjectContext haben können wir die Translate-Methode verwenden, um die Ergebnisse der unsere gespeicherte Prozedur in Entitäten zu übersetzen, die nachverfolgt, und wie gewohnt in EF verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="a726c-116">Once we have an ObjectContext then we can use the Translate method to translate the results of our stored procedure into entities that can be tracked and used in EF as normal.</span></span> <span data-ttu-id="a726c-117">Im folgenden Codebeispiel wird veranschaulicht, dies in Aktion.</span><span class="sxs-lookup"><span data-stu-id="a726c-117">The following code sample demonstrates this in action.</span></span>

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

<span data-ttu-id="a726c-118">Die Translate-Methode akzeptiert den Reader an, dem wir erhalten, wenn wir die Prozedur, ein EntitySet-Namen und eine MergeOption ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="a726c-118">The Translate method accepts the reader that we received when we executed the procedure, an EntitySet name, and a MergeOption.</span></span> <span data-ttu-id="a726c-119">Der EntitySet-Name wird als "DbSet"-Eigenschaft auf Ihrem abgeleiteten Kontext übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="a726c-119">The EntitySet name will be the same as the DbSet property on your derived context.</span></span> <span data-ttu-id="a726c-120">Die Enumeration MergeOption steuert, wie die Ergebnisse verarbeitet werden, wenn die gleiche Entität bereits im Arbeitsspeicher vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="a726c-120">The MergeOption enum controls how results are handled if the same entity already exists in memory.</span></span>

<span data-ttu-id="a726c-121">Hier durchlaufen wir die Sammlung von Blogs, bevor wir dies ist wichtig, dem obigen Code NextResult, aufrufen, da das erste Resultset verarbeitet werden muss, bevor Sie fortfahren, um das nächste Resultset.</span><span class="sxs-lookup"><span data-stu-id="a726c-121">Here we iterate through the collection of blogs before we call NextResult, this is important given the above code because the first result set must be consumed before moving to the next result set.</span></span>

<span data-ttu-id="a726c-122">Nachdem die beiden übersetzt werden Methoden aufgerufen, und klicken Sie dann die Blog und Post-Entitäten genauso wie andere Einrichtungen, die von EF nachverfolgt werden, und daher geändert oder gelöscht und wie gewohnt gespeichert werden kann.</span><span class="sxs-lookup"><span data-stu-id="a726c-122">Once the two translate methods are called then the Blog and Post entities are tracked by EF the same way as any other entity and so can be modified or deleted and saved as normal.</span></span>

>[!NOTE]
> <span data-ttu-id="a726c-123">EF wird keine Zuordnung berücksichtigt, beim Erstellen von Entitäten mit dem die Translate-Methode.</span><span class="sxs-lookup"><span data-stu-id="a726c-123">EF does not take any mapping into account when it creates entities using the Translate method.</span></span> <span data-ttu-id="a726c-124">Es wird einfach die Spaltennamen im Resultset mit Eigenschaftsnamen für die Klassen übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="a726c-124">It will simply match column names in the result set with property names on your classes.</span></span>

>[!NOTE]
> <span data-ttu-id="a726c-125">Wenn Sie lazy Loading aktiviert, haben Zugriff auf die Beiträge-Eigenschaft auf eine der Entitäten Blog dann EF eine mit der Datenbank, um alle Beiträge, verzögert zu laden Verbindung, obwohl wir alle bereits geladen wurde.</span><span class="sxs-lookup"><span data-stu-id="a726c-125">That if you have lazy loading enabled, accessing the posts property on one of the blog entities then EF will connect to the database to lazily load all posts, even though we have already loaded them all.</span></span> <span data-ttu-id="a726c-126">Dies ist da EF nicht bekannt ist, unabhängig davon, ob Sie alle Beiträge geladen haben oder wenn es mehr in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="a726c-126">This is because EF cannot know whether or not you have loaded all posts or if there are more in the database.</span></span> <span data-ttu-id="a726c-127">Wenn Sie dieses Problem zu vermeiden möchten, müssen Sie lazy Loading deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="a726c-127">If you want to avoid this then you will need to disable lazy loading.</span></span>

## <a name="multiple-result-sets-with-configured-in-edmx"></a><span data-ttu-id="a726c-128">Mehrere Resultsets mit der Konfiguration in EDMX-Datei</span><span class="sxs-lookup"><span data-stu-id="a726c-128">Multiple Result Sets with Configured in EDMX</span></span>

>[!NOTE]
> <span data-ttu-id="a726c-129">Sie müssen als Ziel .NET Framework 4.5, um mehrere Resultsets in EDMX-Datei konfigurieren zu können.</span><span class="sxs-lookup"><span data-stu-id="a726c-129">You must target .NET Framework 4.5 to be able to configure multiple result sets in EDMX.</span></span> <span data-ttu-id="a726c-130">Wenn Sie .NET 4.0 verwenden möchten, können Sie die codebasierte-Methode, die im vorherigen Abschnitt gezeigt.</span><span class="sxs-lookup"><span data-stu-id="a726c-130">If you are targeting .NET 4.0 you can use the code-based method shown in the previous section.</span></span>

<span data-ttu-id="a726c-131">Wenn Sie dem EF Designer verwenden, können Sie Ihr Modell auch ändern, damit bekannt ist, Informationen zu den verschiedenen Resultsets, die zurückgegeben werden.</span><span class="sxs-lookup"><span data-stu-id="a726c-131">If you are using the EF Designer, you can also modify your model so that it knows about the different result sets that will be returned.</span></span> <span data-ttu-id="a726c-132">Dabei ist zu wissen, bevor die Seite ist, dass die Tools nicht mehrere Resultsets festgelegt darüber im Klaren, daher Sie die Edmx-Datei manuell bearbeiten müssen.</span><span class="sxs-lookup"><span data-stu-id="a726c-132">One thing to know before hand is that the tooling is not multiple result set aware, so you will need to manually edit the edmx file.</span></span> <span data-ttu-id="a726c-133">Bearbeiten die Edmx-Datei an, wie dies funktioniert, aber sie auch die Überprüfung des Modells in Visual Studio verlieren.</span><span class="sxs-lookup"><span data-stu-id="a726c-133">Editing the edmx file like this will work but it will also break the validation of the model in VS.</span></span> <span data-ttu-id="a726c-134">Und wenn Sie überprüfen, das Modell ob immer Fehler.</span><span class="sxs-lookup"><span data-stu-id="a726c-134">So if you validate your model you will always get errors.</span></span>

-   <span data-ttu-id="a726c-135">Zu diesem Zweck müssen Sie die gespeicherte Prozedur wie bei der ein einzelnes Ergebnis Festlegen der Abfrage aus dem Modell hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="a726c-135">In order to do this you need to add the stored procedure to your model as you would for a single result set query.</span></span>
-   <span data-ttu-id="a726c-136">Sobald Sie dies haben, müssen Sie ein, klicken Sie mit der rechten Maustaste auf das Modell, und wählen Sie **Öffnen mit...**</span><span class="sxs-lookup"><span data-stu-id="a726c-136">Once you have this then you need to right click on your model and select **Open With..**</span></span> <span data-ttu-id="a726c-137">Klicken Sie dann **Xml**</span><span class="sxs-lookup"><span data-stu-id="a726c-137">then **Xml**</span></span>

    ![OpenAs](~/ef6/media/openas.png)

<span data-ttu-id="a726c-139">Sobald Sie haben das Modell im XML-Format geöffnet wird, müssen Sie die folgenden Schritte ausführen:</span><span class="sxs-lookup"><span data-stu-id="a726c-139">Once you have the model opened as XML then you need to do the following steps:</span></span>

-   <span data-ttu-id="a726c-140">Suchen Sie den komplexen Typ und Funktion Import in Ihrem Modell:</span><span class="sxs-lookup"><span data-stu-id="a726c-140">Find the complex type and function import in your model:</span></span>

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

 

-   <span data-ttu-id="a726c-141">Entfernen Sie den komplexen Typ</span><span class="sxs-lookup"><span data-stu-id="a726c-141">Remove the complex type</span></span>
-   <span data-ttu-id="a726c-142">Aktualisieren Sie die Funktion zu importieren, sodass es für Ihre Entitäten, in unserem Fall es zugeordnet ist wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="a726c-142">Update the function import so that it maps to your entities, in our case it will look like the following:</span></span>

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

<span data-ttu-id="a726c-143">Dies weist dem Modell, dass die gespeicherte Prozedur zwei Auflistungen, Blogeinträge und der Post-Einträge zurückgegeben wird.</span><span class="sxs-lookup"><span data-stu-id="a726c-143">This tells the model that the stored procedure will return two collections, one of blog entries and one of post entries.</span></span>

-   <span data-ttu-id="a726c-144">Suchen Sie die Funktion Mapping-Element:</span><span class="sxs-lookup"><span data-stu-id="a726c-144">Find the function mapping element:</span></span>

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

-   <span data-ttu-id="a726c-145">Ersetzen Sie die Ergebnis-Zuordnung durch eine für jede Entität, die zurückgegeben wird, wie z. B. Folgendes ein:</span><span class="sxs-lookup"><span data-stu-id="a726c-145">Replace the result mapping with one for each entity being returned, such as the following:</span></span>

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

<span data-ttu-id="a726c-146">Es ist auch möglich, komplexe Typen, z. B. die, die standardmäßig erstellt das Resultset mit Vorwärtscursor zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="a726c-146">It is also possible to map the result sets to complex types, such as the one created by default.</span></span> <span data-ttu-id="a726c-147">Dazu müssen Sie einen neuen komplexen Typ, statt zu entfernen, erstellen und verwenden Sie die komplexen Typen überall, dass Sie die Namen der Entitäten in den Beispielen oben verwendet haben.</span><span class="sxs-lookup"><span data-stu-id="a726c-147">To do this you create a new complex type, instead of removing them, and use the complex types everywhere that you had used the entity names in the examples above.</span></span>

<span data-ttu-id="a726c-148">Sobald diese Zuordnungen können Sie das Modell speichern und führen Sie den folgenden Code zum Verwenden der gespeicherten Prozedur geändert wurden:</span><span class="sxs-lookup"><span data-stu-id="a726c-148">Once these mappings have been changed then you can save the model and execute the following code to use the stored procedure:</span></span>

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
> <span data-ttu-id="a726c-149">Wenn Sie die Edmx-Datei für Ihr Modell manuell bearbeiten wird sie überschrieben werden, wenn Sie das Modell aus der Datenbank immer neu generieren.</span><span class="sxs-lookup"><span data-stu-id="a726c-149">If you manually edit the edmx file for your model it will be overwritten if you ever regenerate the model from the database.</span></span>

## <a name="summary"></a><span data-ttu-id="a726c-150">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="a726c-150">Summary</span></span>

<span data-ttu-id="a726c-151">Hier haben wir gezeigt, zwei verschiedene Methoden für den Zugriff auf mehrere Sätze unter Verwendung von Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a726c-151">Here we have shown two different methods of accessing multiple result sets using Entity Framework.</span></span> <span data-ttu-id="a726c-152">Beide Angaben sind gleichermaßen gültig ist, je nach Situation und Einstellungen, und Sie sollten auswählen, die für Ihre Situation am besten geeignet ist.</span><span class="sxs-lookup"><span data-stu-id="a726c-152">Both of them are equally valid depending on your situation and preferences and you should choose the one that seems best for your circumstances.</span></span> <span data-ttu-id="a726c-153">Es ist geplant, dass die Unterstützung für mehrere Resultsets, die Gruppen werden in zukünftigen Versionen von Entity Framework verbessert und den Schritten in diesem Dokument nicht mehr notwendig sein werden.</span><span class="sxs-lookup"><span data-stu-id="a726c-153">It is planned that the support for multiple result sets will be improved in future versions of Entity Framework and that performing the steps in this document will no longer be necessary.</span></span>
