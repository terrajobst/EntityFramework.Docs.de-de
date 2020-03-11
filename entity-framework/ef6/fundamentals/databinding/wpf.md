---
title: Datenbindung mit WPF-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 1933988277d3be8fecc02fced3293f2b7f80c901
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414098"
---
# <a name="databinding-with-wpf"></a><span data-ttu-id="16c8a-102">Datenbindung mit WPF</span><span class="sxs-lookup"><span data-stu-id="16c8a-102">Databinding with WPF</span></span>
<span data-ttu-id="16c8a-103">In dieser schrittweisen exemplarischen Vorgehensweise wird gezeigt, wie poco-Typen an WPF-Steuerelemente in einem Master-Detail-Formular gebunden werden.</span><span class="sxs-lookup"><span data-stu-id="16c8a-103">This step-by-step walkthrough shows how to bind POCO types to WPF controls in a “master-detail" form.</span></span> <span data-ttu-id="16c8a-104">Die Anwendung verwendet die Entity Framework-APIs zum Auffüllen von Objekten mit Daten aus der Datenbank, zum Nachverfolgen von Änderungen und zum Beibehalten von Daten in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="16c8a-104">The application uses the Entity Framework APIs to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="16c8a-105">Das Modell definiert zwei Typen, die an einer 1: n-Beziehung beteiligt sind: **Kategorie** (Principal\\Master) und **Product** (abhängige\\Details).</span><span class="sxs-lookup"><span data-stu-id="16c8a-105">The model defines two types that participate in one-to-many relationship: **Category** (principal\\master) and **Product** (dependent\\detail).</span></span> <span data-ttu-id="16c8a-106">Anschließend werden die Visual Studio-Tools verwendet, um die im Modell definierten Typen an die WPF-Steuerelemente zu binden.</span><span class="sxs-lookup"><span data-stu-id="16c8a-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WPF controls.</span></span> <span data-ttu-id="16c8a-107">Das WPF-Daten Bindungs Framework ermöglicht die Navigation zwischen verbundenen Objekten: durch Auswählen von Zeilen in der Master Ansicht wird die Detailansicht mit den entsprechenden untergeordneten Daten aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="16c8a-107">The WPF data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="16c8a-108">Die Screenshots und Code Auflistungen in dieser exemplarischen Vorgehensweise werden aus Visual Studio 2013 erstellt, aber Sie können diese exemplarische Vorgehensweise mit Visual Studio 2012 oder Visual Studio 2010 durcharbeiten.</span><span class="sxs-lookup"><span data-stu-id="16c8a-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a><span data-ttu-id="16c8a-109">Verwenden Sie die Option "Object" zum Erstellen von WPF-Datenquellen.</span><span class="sxs-lookup"><span data-stu-id="16c8a-109">Use the 'Object' Option for Creating WPF Data Sources</span></span>

<span data-ttu-id="16c8a-110">Mit der vorherigen Version von Entity Framework wir die Verwendung der **Daten Bank** Option empfohlen, wenn eine neue Datenquelle basierend auf einem Modell erstellt wird, das mit dem EF-Designer erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="16c8a-110">With previous version of Entity Framework we used to recommend using the **Database** option when creating a new Data Source based on a model created with the EF Designer.</span></span> <span data-ttu-id="16c8a-111">Der Grund hierfür ist, dass der Designer einen Kontext generieren würde, der von ObjectContext-und Entitäts Klassen abgeleitet wurde, die von EntityObject abgeleitet wurden.</span><span class="sxs-lookup"><span data-stu-id="16c8a-111">This was because the designer would generate a context that derived from ObjectContext and entity classes that derived from EntityObject.</span></span> <span data-ttu-id="16c8a-112">Wenn Sie die-Datenbankoption verwenden, können Sie den besten Code für die Interaktion mit dieser API-Oberfläche schreiben.</span><span class="sxs-lookup"><span data-stu-id="16c8a-112">Using the Database option would help you write the best code for interacting with this API surface.</span></span>

<span data-ttu-id="16c8a-113">Die EF-Designer für Visual Studio 2012 und Visual Studio 2013 generieren einen Kontext, der von dbcontext zusammen mit einfachen poco-Entitäts Klassen abgeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="16c8a-113">The EF Designers for Visual Studio 2012 and Visual Studio 2013 generate a context that derives from DbContext together with simple POCO entity classes.</span></span> <span data-ttu-id="16c8a-114">In Visual Studio 2010 empfiehlt es sich, auf eine Code Generierungs Vorlage zu wechseln, die dbcontext verwendet, wie weiter unten in dieser exemplarischen Vorgehensweise beschrieben.</span><span class="sxs-lookup"><span data-stu-id="16c8a-114">With Visual Studio 2010 we recommend swapping to a code generation template that uses DbContext as described later in this walkthrough.</span></span>

<span data-ttu-id="16c8a-115">Wenn Sie die dbcontext-API-Oberfläche verwenden, sollten Sie beim Erstellen einer neuen Datenquelle die- **Objekt** Option verwenden, wie in dieser exemplarischen Vorgehensweise dargestellt.</span><span class="sxs-lookup"><span data-stu-id="16c8a-115">When using the DbContext API surface you should use the **Object** option when creating a new Data Source, as shown in this walkthrough.</span></span>

<span data-ttu-id="16c8a-116">Bei Bedarf können Sie die [auf ObjectContext basierende Codegenerierung](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) für Modelle zurücksetzen, die mit dem EF-Designer erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="16c8a-116">If needed, you can [revert to ObjectContext based code generation](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) for models created with the EF Designer.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="16c8a-117">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="16c8a-117">Pre-Requisites</span></span>

<span data-ttu-id="16c8a-118">Zum Durchführen dieser exemplarischen Vorgehensweise muss Visual Studio 2013, Visual Studio 2012 oder Visual Studio 2010 installiert sein.</span><span class="sxs-lookup"><span data-stu-id="16c8a-118">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="16c8a-119">Wenn Sie Visual Studio 2010 verwenden, müssen Sie auch nuget installieren.</span><span class="sxs-lookup"><span data-stu-id="16c8a-119">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="16c8a-120">Weitere Informationen finden Sie unter [Installieren von nuget](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span><span class="sxs-lookup"><span data-stu-id="16c8a-120">For more information, see [Installing NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).</span></span>  

## <a name="create-the-application"></a><span data-ttu-id="16c8a-121">Erstellen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="16c8a-121">Create the Application</span></span>

-   <span data-ttu-id="16c8a-122">Öffnen Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="16c8a-122">Open Visual Studio</span></span>
-   <span data-ttu-id="16c8a-123">**Datei&gt; Projekt für neue&gt;....**</span><span class="sxs-lookup"><span data-stu-id="16c8a-123">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="16c8a-124">Wählen Sie im linken Bereich **Windows** - und im rechten Bereich **wpfapplikation** aus.</span><span class="sxs-lookup"><span data-stu-id="16c8a-124">Select **Windows** in the left pane and **WPFApplication** in the right pane</span></span>
-   <span data-ttu-id="16c8a-125">Geben Sie die **wpfwiamsample** - als Name ein.</span><span class="sxs-lookup"><span data-stu-id="16c8a-125">Enter **WPFwithEFSample** as the name</span></span>
-   <span data-ttu-id="16c8a-126"> **OK** auswählen</span><span class="sxs-lookup"><span data-stu-id="16c8a-126">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="16c8a-127">Installieren des Entity Framework nuget-Pakets</span><span class="sxs-lookup"><span data-stu-id="16c8a-127">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="16c8a-128">Klicken Sie in Projektmappen-Explorer mit der rechten Maustaste auf das **winformswideratsample** -Projekt.</span><span class="sxs-lookup"><span data-stu-id="16c8a-128">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="16c8a-129">Wählen Sie **nuget-Pakete verwalten... aus.**</span><span class="sxs-lookup"><span data-stu-id="16c8a-129">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="16c8a-130">Wählen Sie im Dialogfeld nuget-Pakete verwalten die Registerkarte **Online** aus, und wählen Sie das Paket **EntityFramework** aus.</span><span class="sxs-lookup"><span data-stu-id="16c8a-130">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="16c8a-131">Klicken Sie auf **Installieren**</span><span class="sxs-lookup"><span data-stu-id="16c8a-131">Click **Install**</span></span>  
    >[!NOTE]
    > <span data-ttu-id="16c8a-132">Zusätzlich zur EntityFramework-Assembly wird auch ein Verweis auf "System. ComponentModel. DataAnnotations" hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="16c8a-132">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="16c8a-133">Wenn das Projekt einen Verweis auf System. Data. Entity enthält, wird es entfernt, wenn das EntityFramework-Paket installiert wird.</span><span class="sxs-lookup"><span data-stu-id="16c8a-133">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="16c8a-134">Die System. Data. Entity-Assembly wird für Entity Framework 6-Anwendungen nicht mehr verwendet.</span><span class="sxs-lookup"><span data-stu-id="16c8a-134">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="define-a-model"></a><span data-ttu-id="16c8a-135">Definieren eines Modells</span><span class="sxs-lookup"><span data-stu-id="16c8a-135">Define a Model</span></span>

<span data-ttu-id="16c8a-136">In dieser exemplarischen Vorgehensweise können Sie ein Modell mithilfe von Code First oder dem EF-Designer implementieren.</span><span class="sxs-lookup"><span data-stu-id="16c8a-136">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="16c8a-137">Führen Sie einen der beiden folgenden Abschnitte aus.</span><span class="sxs-lookup"><span data-stu-id="16c8a-137">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="16c8a-138">Option 1: Definieren eines Modells mit Code First</span><span class="sxs-lookup"><span data-stu-id="16c8a-138">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="16c8a-139">In diesem Abschnitt wird gezeigt, wie ein Modell und die zugehörige Datenbank mithilfe Code First erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="16c8a-139">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="16c8a-140">Fahren Sie mit dem nächsten Abschnitt fort (**Option 2: Definieren eines Modells mithilfe Database First)** , wenn Sie stattdessen Database First verwenden möchten, um das Modell aus einer Datenbank mit dem EF-Designer umzukehren.</span><span class="sxs-lookup"><span data-stu-id="16c8a-140">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="16c8a-141">Wenn Sie Code First Entwicklung verwenden, schreiben Sie in der Regel .NET Framework Klassen, die ihr konzeptionelles (Domänen-) Modell definieren.</span><span class="sxs-lookup"><span data-stu-id="16c8a-141">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="16c8a-142">Fügen Sie der **wpfwiderfsample** -Klasse eine neue Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="16c8a-142">Add a new class to the **WPFwithEFSample:**</span></span>
    -   <span data-ttu-id="16c8a-143">Klicken Sie mit der rechten Maustaste auf den Projektnamen.</span><span class="sxs-lookup"><span data-stu-id="16c8a-143">Right-click on the project name</span></span>
    -   <span data-ttu-id="16c8a-144">Wählen Sie **Hinzufügen**und dann **Neues Element** aus.</span><span class="sxs-lookup"><span data-stu-id="16c8a-144">Select **Add**, then **New Item**</span></span>
    -   <span data-ttu-id="16c8a-145">Wählen Sie **Klasse** aus, und geben Sie **Product** als Klassennamen ein.</span><span class="sxs-lookup"><span data-stu-id="16c8a-145">Select **Class** and enter **Product** for the class name</span></span>
-   <span data-ttu-id="16c8a-146">Ersetzen Sie die **Product** -Klassendefinition durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="16c8a-146">Replace the **Product** class definition with the following code:</span></span>

``` csharp
    namespace WPFwithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }

-   Add a **Category** class with the following definition:

    using System.Collections.ObjectModel;

    namespace WPFwithEFSample
    {
        public class Category
        {
            public Category()
            {
                this.Products = new ObservableCollection<Product>();
            }

            public int CategoryId { get; set; }
            public string Name { get; set; }

            public virtual ObservableCollection<Product> Products { get; private set; }
        }
    }
```

<span data-ttu-id="16c8a-147">Die **Products** -Eigenschaft der **Category** -Klasse und der **Category** -Eigenschaft in der **Product** -Klasse sind Navigations Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="16c8a-147">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="16c8a-148">In Entity Framework bieten Navigations Eigenschaften eine Möglichkeit, eine Beziehung zwischen zwei Entitäts Typen zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="16c8a-148">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="16c8a-149">Zusätzlich zum Definieren von Entitäten müssen Sie eine Klasse definieren, die von dbcontext abgeleitet ist und dbset&lt;TEntity&gt; Eigenschaften verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="16c8a-149">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="16c8a-150">Mit den Eigenschaften von dbset&lt;TEntity&gt; wird der Kontext informiert, welche Typen Sie in das Modell einschließen möchten.</span><span class="sxs-lookup"><span data-stu-id="16c8a-150">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="16c8a-151">Eine Instanz des abgeleiteten dbcontext-Typs verwaltet die Entitäts Objekte zur Laufzeit. dazu gehören das Auffüllen von Objekten mit Daten aus einer Datenbank, die Änderungs Nachverfolgung und das Beibehalten von Daten in der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="16c8a-151">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="16c8a-152">Fügen Sie dem Projekt eine neue **productcontext** -Klasse mit der folgenden Definition hinzu:</span><span class="sxs-lookup"><span data-stu-id="16c8a-152">Add a new **ProductContext** class to the project with the following definition:</span></span>

``` csharp
    using System.Data.Entity;

    namespace WPFwithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

<span data-ttu-id="16c8a-153">Kompilieren Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="16c8a-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="16c8a-154">Option 2: Definieren eines Modells mit Database First</span><span class="sxs-lookup"><span data-stu-id="16c8a-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="16c8a-155">In diesem Abschnitt wird gezeigt, wie Sie mit Database First das Modell aus einer Datenbank mit dem EF-Designer rückgängig machen.</span><span class="sxs-lookup"><span data-stu-id="16c8a-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="16c8a-156">Wenn Sie den vorherigen Abschnitt (**Option 1: Definieren eines Modells mit Code First)** abgeschlossen haben, überspringen Sie diesen Abschnitt, und wechseln Sie direkt zum Abschnitt **Lazy Load** .</span><span class="sxs-lookup"><span data-stu-id="16c8a-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="16c8a-157">Erstellen einer vorhandenen Datenbank</span><span class="sxs-lookup"><span data-stu-id="16c8a-157">Create an Existing Database</span></span>

<span data-ttu-id="16c8a-158">Wenn Sie eine vorhandene Datenbank als Ziel haben, wird Sie in der Regel bereits erstellt, aber in dieser exemplarischen Vorgehensweise müssen wir eine Datenbank erstellen, auf die zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="16c8a-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="16c8a-159">Der Datenbankserver, der mit Visual Studio installiert wird, unterscheidet sich abhängig von der installierten Version von Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="16c8a-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="16c8a-160">Wenn Sie Visual Studio 2010 verwenden, erstellen Sie eine SQL Express-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="16c8a-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="16c8a-161">Wenn Sie Visual Studio 2012 verwenden, erstellen Sie eine [localdb](https://msdn.microsoft.com/library/hh510202.aspx) -Datenbank.</span><span class="sxs-lookup"><span data-stu-id="16c8a-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="16c8a-162">Nun generieren wir die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="16c8a-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="16c8a-163">**&gt; Server-Explorer anzeigen**</span><span class="sxs-lookup"><span data-stu-id="16c8a-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="16c8a-164">Klicken Sie mit der rechten Maustaste auf **Datenverbindungen,&gt; Verbindung hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="16c8a-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="16c8a-165">Wenn Sie über Server-Explorer keine Verbindung mit einer Datenbank hergestellt haben, müssen Sie Microsoft SQL Server als Datenquelle auswählen.</span><span class="sxs-lookup"><span data-stu-id="16c8a-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![Datenquelle ändern](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="16c8a-167">Stellen Sie abhängig von der installierten Verbindung entweder localdb oder SQL Express her, und geben Sie **Produkte** als Datenbanknamen ein.</span><span class="sxs-lookup"><span data-stu-id="16c8a-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![Verbindung localdb hinzufügen](~/ef6/media/addconnectionlocaldb.png)

    ![Verbindung hinzufügen Express](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="16c8a-170">Wählen Sie **OK** aus, und Sie werden gefragt, ob Sie eine neue Datenbank erstellen möchten, und wählen Sie **Ja** aus.</span><span class="sxs-lookup"><span data-stu-id="16c8a-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![Erstellen einer Datenbank](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="16c8a-172">Die neue Datenbank wird nun in Server-Explorer angezeigt, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **neue Abfrage** aus.</span><span class="sxs-lookup"><span data-stu-id="16c8a-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="16c8a-173">Kopieren Sie den folgenden SQL-Befehl in die neue Abfrage, klicken Sie mit der rechten Maustaste auf die Abfrage, und wählen Sie **Ausführen** .</span><span class="sxs-lookup"><span data-stu-id="16c8a-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
    CREATE TABLE [dbo].[Categories] (
        [CategoryId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        CONSTRAINT [PK_dbo.Categories] PRIMARY KEY ([CategoryId])
    )

    CREATE TABLE [dbo].[Products] (
        [ProductId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        [CategoryId] [int] NOT NULL,
        CONSTRAINT [PK_dbo.Products] PRIMARY KEY ([ProductId])
    )

    CREATE INDEX [IX_CategoryId] ON [dbo].[Products]([CategoryId])

    ALTER TABLE [dbo].[Products] ADD CONSTRAINT [FK_dbo.Products_dbo.Categories_CategoryId] FOREIGN KEY ([CategoryId]) REFERENCES [dbo].[Categories] ([CategoryId]) ON DELETE CASCADE
```

#### <a name="reverse-engineer-model"></a><span data-ttu-id="16c8a-174">Reverse Engineering-Modell</span><span class="sxs-lookup"><span data-stu-id="16c8a-174">Reverse Engineer Model</span></span>

<span data-ttu-id="16c8a-175">Wir verwenden Entity Framework Designer, die als Teil von Visual Studio enthalten ist, um das Modell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="16c8a-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="16c8a-176">**Projekt&gt; neues Element hinzufügen...**</span><span class="sxs-lookup"><span data-stu-id="16c8a-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="16c8a-177">Wählen Sie im linken Menü **Daten** aus, und klicken Sie dann auf **ADO.NET Entity Data Model**</span><span class="sxs-lookup"><span data-stu-id="16c8a-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="16c8a-178">Geben Sie **ProductModel** als Name ein, und klicken Sie auf **OK** .</span><span class="sxs-lookup"><span data-stu-id="16c8a-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="16c8a-179">Dadurch wird der **Entity Data Model-Assistent** gestartet.</span><span class="sxs-lookup"><span data-stu-id="16c8a-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="16c8a-180">Wählen Sie **aus Datenbank generieren aus** , und klicken Sie auf **weiter**</span><span class="sxs-lookup"><span data-stu-id="16c8a-180">Select **Generate from Database** and click **Next**</span></span>

    ![Auswählen von Modellinhalten](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="16c8a-182">Wählen Sie die Verbindung mit der Datenbank, die Sie im ersten Abschnitt erstellt haben, geben Sie **productcontext** als Namen der Verbindungs Zeichenfolge ein, und klicken Sie auf **weiter** .</span><span class="sxs-lookup"><span data-stu-id="16c8a-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![Verbindung auswählen](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="16c8a-184">Aktivieren Sie das Kontrollkästchen neben "Tables", um alle Tabellen zu importieren, und klicken Sie auf "Fertigstellen"</span><span class="sxs-lookup"><span data-stu-id="16c8a-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![Objekte auswählen](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="16c8a-186">Sobald der Reverse-Engineering-Prozess abgeschlossen ist, wird das neue Modell dem Projekt hinzugefügt und geöffnet, damit Sie es im Entity Framework Designer anzeigen können.</span><span class="sxs-lookup"><span data-stu-id="16c8a-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="16c8a-187">Eine APP. config-Datei wurde dem Projekt auch mit den Verbindungsdetails für die Datenbank hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="16c8a-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="16c8a-188">Weitere Schritte in Visual Studio 2010</span><span class="sxs-lookup"><span data-stu-id="16c8a-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="16c8a-189">Wenn Sie in Visual Studio 2010 arbeiten, müssen Sie den EF-Designer aktualisieren, damit die EF6-Codegenerierung verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="16c8a-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="16c8a-190">Klicken Sie mit der rechten Maustaste auf eine leere Stelle des Modells im EF-Designer, und wählen Sie **Code Generierungs Element hinzufügen... aus.**</span><span class="sxs-lookup"><span data-stu-id="16c8a-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="16c8a-191">Wählen Sie im linken Menü **Online Vorlagen** aus, und suchen Sie nach **dbcontext** .</span><span class="sxs-lookup"><span data-stu-id="16c8a-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="16c8a-192">Wählen Sie den **EF 6. x dbcontext Generator für C\#aus,** geben Sie **ProductModel** als Name ein, und klicken Sie auf hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="16c8a-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="16c8a-193">Aktualisieren der Codegenerierung für die Datenbindung</span><span class="sxs-lookup"><span data-stu-id="16c8a-193">Updating code generation for data binding</span></span>

<span data-ttu-id="16c8a-194">EF generiert Code aus dem Modell mithilfe von T4-Vorlagen.</span><span class="sxs-lookup"><span data-stu-id="16c8a-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="16c8a-195">Die Vorlagen, die in Visual Studio enthalten sind oder aus der Visual Studio Gallery heruntergeladen wurden, sind für die allgemeine Verwendung vorgesehen.</span><span class="sxs-lookup"><span data-stu-id="16c8a-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="16c8a-196">Dies bedeutet, dass die von diesen Vorlagen generierten Entitäten einfache ICollection-&lt;t-&gt; Eigenschaften haben.</span><span class="sxs-lookup"><span data-stu-id="16c8a-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="16c8a-197">Wenn Sie jedoch die Datenbindung mithilfe von WPF durchführen, ist es wünschenswert, **ObservableCollection** für Auflistungs Eigenschaften zu verwenden, damit WPF Änderungen an den Auflistungen nachverfolgen kann.</span><span class="sxs-lookup"><span data-stu-id="16c8a-197">However, when doing data binding using WPF it is desirable to use **ObservableCollection** for collection properties so that WPF can keep track of changes made to the collections.</span></span> <span data-ttu-id="16c8a-198">Zu diesem Zweck werden die Vorlagen für die Verwendung von ObservableCollection geändert.</span><span class="sxs-lookup"><span data-stu-id="16c8a-198">To this end we will to modify the templates to use ObservableCollection.</span></span>

-   <span data-ttu-id="16c8a-199">Öffnen Sie die **Projektmappen-Explorer** , und suchen Sie nach der Datei **ProductModel. edmx.**</span><span class="sxs-lookup"><span data-stu-id="16c8a-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="16c8a-200">Suchen Sie die **ProductModel.tt** -Datei, die unter der Datei "ProductModel. edmx" eingefügt wird.</span><span class="sxs-lookup"><span data-stu-id="16c8a-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![WPF-Produktmodell Vorlage](~/ef6/media/wpfproductmodeltemplate.png)

-   <span data-ttu-id="16c8a-202">Doppelklicken Sie auf die Datei ProductModel.tt, um Sie im Visual Studio-Editor zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="16c8a-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="16c8a-203">Suchen und ersetzen Sie die beiden Vorkommen von "**ICollection**" durch "**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="16c8a-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="16c8a-204">Diese befinden sich ungefähr in den Zeilen 296 und 484.</span><span class="sxs-lookup"><span data-stu-id="16c8a-204">These are located approximately at lines 296 and 484.</span></span>
-   <span data-ttu-id="16c8a-205">Suchen und ersetzen Sie das erste Vorkommen von "**HashSet**" durch "**ObservableCollection**".</span><span class="sxs-lookup"><span data-stu-id="16c8a-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableCollection**”.</span></span> <span data-ttu-id="16c8a-206">Dieses Vorkommen befindet sich ungefähr in Zeile 50.</span><span class="sxs-lookup"><span data-stu-id="16c8a-206">This occurrence is located approximately at line 50.</span></span> <span data-ttu-id="16c8a-207">Ersetzen **Sie nicht** das zweite Vorkommen des Hashsets, das später im Code gefunden wurde.</span><span class="sxs-lookup"><span data-stu-id="16c8a-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="16c8a-208">Suchen und ersetzen Sie das einzige Vorkommen von "**System. Collections. Generic**" durch "**System. Collections. ObjectModel**".</span><span class="sxs-lookup"><span data-stu-id="16c8a-208">Find and replace the only occurrence of “**System.Collections.Generic**” with “**System.Collections.ObjectModel**”.</span></span> <span data-ttu-id="16c8a-209">Dies befindet sich ungefähr in Zeile 424.</span><span class="sxs-lookup"><span data-stu-id="16c8a-209">This is located approximately at line 424.</span></span>
-   <span data-ttu-id="16c8a-210">Speichern Sie die Datei ProductModel.tt.</span><span class="sxs-lookup"><span data-stu-id="16c8a-210">Save the ProductModel.tt file.</span></span> <span data-ttu-id="16c8a-211">Dies sollte bewirken, dass der Code für Entitäten erneut generiert wird.</span><span class="sxs-lookup"><span data-stu-id="16c8a-211">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="16c8a-212">Wenn der Code nicht automatisch neu generiert wird, klicken Sie mit der rechten Maustaste auf ProductModel.tt, und wählen Sie "benutzerdefiniertes Tool ausführen".</span><span class="sxs-lookup"><span data-stu-id="16c8a-212">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="16c8a-213">Wenn Sie nun die Category.cs-Datei öffnen (die unter ProductModel.tt geschachtelte ist), sollten Sie sehen, dass die Products-Sammlung den Typ **ObservableCollection&lt;Product&gt;** hat.</span><span class="sxs-lookup"><span data-stu-id="16c8a-213">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableCollection&lt;Product&gt;**.</span></span>

<span data-ttu-id="16c8a-214">Kompilieren Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="16c8a-214">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="16c8a-215">Lazy Loading</span><span class="sxs-lookup"><span data-stu-id="16c8a-215">Lazy Loading</span></span>

<span data-ttu-id="16c8a-216">Die **Products** -Eigenschaft der **Category** -Klasse und der **Category** -Eigenschaft in der **Product** -Klasse sind Navigations Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="16c8a-216">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="16c8a-217">In Entity Framework bieten Navigations Eigenschaften eine Möglichkeit, eine Beziehung zwischen zwei Entitäts Typen zu navigieren.</span><span class="sxs-lookup"><span data-stu-id="16c8a-217">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="16c8a-218">EF bietet Ihnen die Möglichkeit, Verwandte Entitäten aus der Datenbank automatisch zu laden, wenn Sie zum ersten Mal auf die Navigations Eigenschaft zugreifen.</span><span class="sxs-lookup"><span data-stu-id="16c8a-218">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="16c8a-219">Beachten Sie bei dieser Art von laden (Lazy Loading), dass beim ersten Zugriff auf jede Navigations Eigenschaft eine separate Abfrage für die Datenbank ausgeführt wird, wenn der Inhalt nicht bereits im Kontext vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="16c8a-219">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="16c8a-220">Wenn poco-Entitäts Typen verwendet werden, erreicht EF Lazy Loading durch Erstellen von Instanzen abgeleiteter Proxy Typen während der Laufzeit und das anschließende Überschreiben virtueller Eigenschaften in den Klassen, um den Lade Hook hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="16c8a-220">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="16c8a-221">Wenn Sie Lazy Loading verknüpfter Objekte abrufen möchten, müssen Sie die Navigations Eigenschaften Getter als **Public** und **Virtual** (**Overridable** in Visual Basic) deklarieren, und die Klasse darf nicht **versiegelt** sein (**nodeverridable** in Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="16c8a-221">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="16c8a-222">Bei Verwendung Database First Navigations Eigenschaften automatisch als virtuell fest, um Lazy Loading zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="16c8a-222">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="16c8a-223">Im Code First Abschnitt haben wir entschieden, die Navigations Eigenschaften aus demselben Grund virtuell zu machen.</span><span class="sxs-lookup"><span data-stu-id="16c8a-223">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="16c8a-224">Binden von Objekten an Steuerelemente</span><span class="sxs-lookup"><span data-stu-id="16c8a-224">Bind Object to Controls</span></span>

<span data-ttu-id="16c8a-225">Fügen Sie die Klassen, die im Modell definiert sind, als Datenquellen für diese WPF-Anwendung hinzu.</span><span class="sxs-lookup"><span data-stu-id="16c8a-225">Add the classes that are defined in the model as data sources for this WPF application.</span></span>

-   <span data-ttu-id="16c8a-226">Doppelklicken Sie in Projektmappen-Explorer auf die Datei **MainWindow. XAML** , um das Hauptformular zu öffnen.</span><span class="sxs-lookup"><span data-stu-id="16c8a-226">Double-click **MainWindow.xaml** in Solution Explorer to open the main form</span></span>
-   <span data-ttu-id="16c8a-227">Wählen Sie im Hauptmenü **Projekt&gt; neue Datenquelle hinzufügen aus.**</span><span class="sxs-lookup"><span data-stu-id="16c8a-227">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="16c8a-228">(in Visual Studio 2010 müssen Sie **Daten&gt; neue Datenquelle hinzufügen...** )</span><span class="sxs-lookup"><span data-stu-id="16c8a-228">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="16c8a-229">Wählen Sie im Fenster Datenquelle auswählen die Option **Objekt** aus, und klicken Sie auf **weiter** .</span><span class="sxs-lookup"><span data-stu-id="16c8a-229">In the Choose a Data Source Typewindow, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="16c8a-230">Erweitern Sie im Dialogfeld Wählen Sie die Datenobjekte aus die **wpfwiderf Sample** - zweimal, und wählen Sie **Kategorie** aus.</span><span class="sxs-lookup"><span data-stu-id="16c8a-230">In the Select the Data Objects dialog, unfold the **WPFwithEFSample** two times and select **Category**</span></span>  
    <span data-ttu-id="16c8a-231">*Die **Produkt** Datenquelle muss nicht ausgewählt werden, da wir Sie über die **Produkt**Eigenschaft der **Kategorie** Datenquelle erhalten.*</span><span class="sxs-lookup"><span data-stu-id="16c8a-231">*There is no need to select the **Product** data source, because we will get to it through the **Product**’s property on the **Category** data source*</span></span>  

    ![Datenobjekte auswählen](~/ef6/media/selectdataobjects.png)

-   <span data-ttu-id="16c8a-233">Klicken Sie auf **Fertig stellen**.</span><span class="sxs-lookup"><span data-stu-id="16c8a-233">Click **Finish.**</span></span>
-   <span data-ttu-id="16c8a-234">Das Fenster Datenquellen wird neben dem Fenster MainWindow. XAML geöffnet *, wenn das Fenster Datenquellen nicht angezeigt wird, wählen Sie **Ansicht-&gt; andere Windows-&gt; Datenquellen***  aus.</span><span class="sxs-lookup"><span data-stu-id="16c8a-234">The Data Sources window is opened next to the MainWindow.xaml window *If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources***</span></span>
-   <span data-ttu-id="16c8a-235">Klicken Sie auf das anheft Symbol, damit das Fenster Datenquellen nicht automatisch ausgeblendet wird.</span><span class="sxs-lookup"><span data-stu-id="16c8a-235">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="16c8a-236">Sie müssen möglicherweise auf die Schaltfläche "Aktualisieren" klicken, wenn das Fenster bereits sichtbar war.</span><span class="sxs-lookup"><span data-stu-id="16c8a-236">You may need to hit the refresh button if the window was already visible.</span></span>

    ![Projektmappen-Explorer](~/ef6/media/datasources.png)

-   <span data-ttu-id="16c8a-238">Wählen Sie die **kategoriedatenquelle** aus, und ziehen Sie Sie auf das Formular.</span><span class="sxs-lookup"><span data-stu-id="16c8a-238">Select the **Category** data source and drag it on the form.</span></span>

<span data-ttu-id="16c8a-239">Folgendes ist aufgetreten, als wir diese Quelle gezogen haben:</span><span class="sxs-lookup"><span data-stu-id="16c8a-239">The following happened when we dragged this source:</span></span>

-   <span data-ttu-id="16c8a-240">Die **categoryviewsource** -Ressource und das **categorydatagrid-** Steuerelement wurden zu XAML hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="16c8a-240">The **categoryViewSource** resource and the **categoryDataGrid** control were added to XAML</span></span> 
-   <span data-ttu-id="16c8a-241">Die DataContext-Eigenschaft des übergeordneten Grid-Elements wurde auf "{statikresource **categoryviewsource** }" festgelegt.</span><span class="sxs-lookup"><span data-stu-id="16c8a-241">The DataContext property on the parent Grid element was set to "{StaticResource **categoryViewSource** }".</span></span><span data-ttu-id="16c8a-242"> Die **categoryviewsource** -Ressource dient als Bindungs Quelle für das äußere\\übergeordnete Raster Element.</span><span class="sxs-lookup"><span data-stu-id="16c8a-242"> The **categoryViewSource** resource serves as a binding source for the outer\\parent Grid element.</span></span> <span data-ttu-id="16c8a-243">Die inneren Raster Elemente erben dann den DataContext-Wert vom übergeordneten Raster (die ItemsSource-Eigenschaft von categorydatagrid ist auf "{Binding}" festgelegt).</span><span class="sxs-lookup"><span data-stu-id="16c8a-243">The inner Grid elements then inherit the DataContext value from the parent Grid (the categoryDataGrid’s ItemsSource property is set to "{Binding}")</span></span>

``` xml
    <Window.Resources>
        <CollectionViewSource x:Key="categoryViewSource"
                                d:DesignSource="{d:DesignInstance {x:Type local:Category}, CreateList=True}"/>
    </Window.Resources>
    <Grid DataContext="{StaticResource categoryViewSource}">
        <DataGrid x:Name="categoryDataGrid" AutoGenerateColumns="False" EnableRowVirtualization="True"
                    ItemsSource="{Binding}" Margin="13,13,43,191"
                    RowDetailsVisibilityMode="VisibleWhenSelected">
            <DataGrid.Columns>
                <DataGridTextColumn x:Name="categoryIdColumn" Binding="{Binding CategoryId}"
                                    Header="Category Id" Width="SizeToHeader"/>
                <DataGridTextColumn x:Name="nameColumn" Binding="{Binding Name}"
                                    Header="Name" Width="SizeToHeader"/>
            </DataGrid.Columns>
        </DataGrid>
    </Grid>
```

## <a name="adding-a-details-grid"></a><span data-ttu-id="16c8a-244">Hinzufügen eines Detail Rasters</span><span class="sxs-lookup"><span data-stu-id="16c8a-244">Adding a Details Grid</span></span>

<span data-ttu-id="16c8a-245">Nachdem wir nun über ein Raster zum Anzeigen von Kategorien verfügen, können wir ein Detail Raster hinzufügen, um die zugeordneten Produkte anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="16c8a-245">Now that we have a grid to display Categories let's add a details grid to display the associated Products.</span></span>

-   <span data-ttu-id="16c8a-246">Wählen Sie unter der **Kategorie** Datenquelle die Eigenschaft **Produkte** aus, und ziehen Sie Sie auf das Formular.</span><span class="sxs-lookup"><span data-stu-id="16c8a-246">Select the **Products** property from under the **Category** data source and drag it on the form.</span></span>
    -   <span data-ttu-id="16c8a-247">Die **categoryproductviewsource** -Ressource und das **productdatagrid-** Raster werden XAML hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="16c8a-247">The **categoryProductsViewSource** resource and **productDataGrid** grid are added to XAML</span></span>
    -   <span data-ttu-id="16c8a-248">Der Bindungs Pfad für diese Ressource ist auf Products festgelegt.</span><span class="sxs-lookup"><span data-stu-id="16c8a-248">The binding path for this resource is set to Products</span></span>
    -   <span data-ttu-id="16c8a-249">WPF-Daten Bindungs Framework stellt sicher, dass nur Produkte im Zusammenhang mit der ausgewählten Kategorie in **productdatagrid** angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="16c8a-249">WPF data-binding framework ensures that only Products related to the selected Category show up in **productDataGrid**</span></span>
-   <span data-ttu-id="16c8a-250">Ziehen Sie die **Schaltfläche** aus der Toolbox auf das Formular.</span><span class="sxs-lookup"><span data-stu-id="16c8a-250">From the Toolbox, drag **Button** on to the form.</span></span> <span data-ttu-id="16c8a-251">Legen Sie die **Name** -Eigenschaft auf **ButtonSave** und die **Content** -Eigenschaft auf **Save**fest.</span><span class="sxs-lookup"><span data-stu-id="16c8a-251">Set the **Name** property to **buttonSave** and the **Content** property to **Save**.</span></span>

<span data-ttu-id="16c8a-252">Das Formular sollte in etwa wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="16c8a-252">The form should look similar to this:</span></span>

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a><span data-ttu-id="16c8a-254">Hinzufügen von Code, der die Daten Interaktion verarbeitet</span><span class="sxs-lookup"><span data-stu-id="16c8a-254">Add Code that Handles Data Interaction</span></span>

<span data-ttu-id="16c8a-255">Es ist an der Zeit, dem Hauptfenster einige Ereignishandler hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="16c8a-255">It's time to add some event handlers to the main window.</span></span>

-   <span data-ttu-id="16c8a-256">Klicken Sie im XAML-Fenster auf das **&lt;Window** -Element, damit das Hauptfenster ausgewählt wird.</span><span class="sxs-lookup"><span data-stu-id="16c8a-256">In the XAML window, click on the **&lt;Window** element, this selects the main window</span></span>
-   <span data-ttu-id="16c8a-257">Wählen Sie im **Eigenschaften** Fenster oben rechts **Ereignisse** aus, und doppelklicken Sie dann auf das Textfeld rechts neben der **geladenen** Bezeichnung.</span><span class="sxs-lookup"><span data-stu-id="16c8a-257">In the **Properties** window choose **Events** at the top right, then double-click the text box to right of the **Loaded** label</span></span>

    ![Hauptfenster Eigenschaften](~/ef6/media/mainwindowproperties.png)

-   <span data-ttu-id="16c8a-259">Fügen Sie auch das Click-Ereignis für die Schaltfläche **Speichern** hinzu, indem **Sie** im Designer auf die Schaltfläche Speichern doppelklicken.</span><span class="sxs-lookup"><span data-stu-id="16c8a-259">Also add the **Click** event for the **Save** button by double-clicking the Save button in the designer.</span></span> 

<span data-ttu-id="16c8a-260">Dadurch gelangen Sie zum Code Behind für das Formular. nun bearbeiten wir den Code so, dass der productcontext zum Ausführen von Datenzugriff verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="16c8a-260">This brings you to the code behind for the form, we'll now edit the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="16c8a-261">Aktualisieren Sie den Code für das MainWindow, wie unten gezeigt.</span><span class="sxs-lookup"><span data-stu-id="16c8a-261">Update the code for the MainWindow as shown below.</span></span>

<span data-ttu-id="16c8a-262">Der Code deklariert eine Instanz von **productcontext**mit langer Laufzeit.</span><span class="sxs-lookup"><span data-stu-id="16c8a-262">The code declares a long-running instance of **ProductContext**.</span></span> <span data-ttu-id="16c8a-263">Das **productcontext** -Objekt wird verwendet, um Daten abzufragen und in der Datenbank zu speichern.</span><span class="sxs-lookup"><span data-stu-id="16c8a-263">The **ProductContext** object is used to query and save data to the database.</span></span> <span data-ttu-id="16c8a-264">Der Löschvorgang **()** auf der **productcontext** -Instanz wird dann von der überschriebenen **OnClosing** -Methode aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="16c8a-264">The **Dispose()** on the **ProductContext** instance is then called from the overridden **OnClosing** method.</span></span><span data-ttu-id="16c8a-265"> Die Code Kommentare enthalten Details zum Funktionsumfang des Codes.</span><span class="sxs-lookup"><span data-stu-id="16c8a-265"> The code comments provide details about what the code does.</span></span>

``` csharp
    using System.Data.Entity;
    using System.Linq;
    using System.Windows;

    namespace WPFwithEFSample
    {
        public partial class MainWindow : Window
        {
            private ProductContext _context = new ProductContext();
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                System.Windows.Data.CollectionViewSource categoryViewSource =
                    ((System.Windows.Data.CollectionViewSource)(this.FindResource("categoryViewSource")));

                // Load is an extension method on IQueryable,
                // defined in the System.Data.Entity namespace.
                // This method enumerates the results of the query,
                // similar to ToList but without creating a list.
                // When used with Linq to Entities this method
                // creates entity objects and adds them to the context.
                _context.Categories.Load();

                // After the data is loaded call the DbSet<T>.Local property
                // to use the DbSet<T> as a binding source.
                categoryViewSource.Source = _context.Categories.Local;
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                // When you delete an object from the related entities collection
                // (in this case Products), the Entity Framework doesn’t mark
                // these child entities as deleted.
                // Instead, it removes the relationship between the parent and the child
                // by setting the parent reference to null.
                // So we manually have to delete the products
                // that have a Category reference set to null.

                // The following code uses LINQ to Objects
                // against the Local collection of Products.
                // The ToList call is required because otherwise the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can use LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                _context.SaveChanges();
                // Refresh the grids so the database generated values show up.
                this.categoryDataGrid.Items.Refresh();
                this.productsDataGrid.Items.Refresh();
            }

            protected override void OnClosing(System.ComponentModel.CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }

    }
```

## <a name="test-the-wpf-application"></a><span data-ttu-id="16c8a-266">Testen der WPF-Anwendung</span><span class="sxs-lookup"><span data-stu-id="16c8a-266">Test the WPF Application</span></span>

-   <span data-ttu-id="16c8a-267">Kompilieren Sie die Anwendung, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="16c8a-267">Compile and run the application.</span></span> <span data-ttu-id="16c8a-268">Wenn Sie Code First verwendet haben, werden Sie feststellen, dass eine **wpfwitef Sample. productcontext** -Datenbank für Sie erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="16c8a-268">If you used Code First, then you will see that a **WPFwithEFSample.ProductContext** database is created for you.</span></span>
-   <span data-ttu-id="16c8a-269">Geben Sie einen Kategorienamen im oberen Raster ein, und die Produktnamen im unteren Raster *geben in den ID-Spalten nichts ein, da der Primärschlüssel von der Datenbank generiert wird* .</span><span class="sxs-lookup"><span data-stu-id="16c8a-269">Enter a category name in the top grid and product names in the bottom grid *Do not enter anything in ID columns, because the primary key is generated by the database*</span></span>

    ![Hauptfenster mit neuen Kategorien und Produkten](~/ef6/media/screen1.png)

-   <span data-ttu-id="16c8a-271">Klicken Sie auf die Schaltfläche **Speichern** , um die Daten in der Datenbank zu speichern</span><span class="sxs-lookup"><span data-stu-id="16c8a-271">Press the **Save** button to save the data to the database</span></span>

<span data-ttu-id="16c8a-272">Nach dem Aufrufe von " **SaveChanges ()** " von dbcontext werden die IDs mit den von der Datenbank generierten Werten aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="16c8a-272">After the call to DbContext’s **SaveChanges()**, the IDs are populated with the database generated values.</span></span> <span data-ttu-id="16c8a-273">Da wir " **Refresh ()** " nach " **SaveChanges ()** " aufgerufen haben, werden die **DataGrid-** Steuerelemente ebenfalls mit den neuen Werten aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="16c8a-273">Because we called **Refresh()** after **SaveChanges()** the **DataGrid** controls are updated with the new values as well.</span></span>

![Hauptfenster mit aufgefüllten IDs](~/ef6/media/screen2.png)

## <a name="additional-resources"></a><span data-ttu-id="16c8a-275">Weitere Ressourcen</span><span class="sxs-lookup"><span data-stu-id="16c8a-275">Additional Resources</span></span>

<span data-ttu-id="16c8a-276">Weitere Informationen zur Datenbindung an Auflistungen mithilfe von WPF finden Sie in [diesem Thema](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) in der WPF-Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="16c8a-276">To learn more about data binding to collections using WPF, see [this topic](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) in the WPF documentation.</span></span>  
